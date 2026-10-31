# 11. Docker, VPS and Backend Networking Architecture

## Request path
```text
Browser
  | DNS: app.example -> VPS public IP
  | TCP 443 + TLS
VPS/cloud firewall
  |
Nginx reverse proxy (public ingress)
  | private Docker network
Express API :3000
  | private Docker network
MongoDB :27017
```

Only intended ingress should be published. MongoDB normally needs no public host port.

## Network namespaces and service discovery
Each container has its own network namespace, including loopback. `localhost` inside Express means Express itself. On a shared Compose network, Docker DNS resolves the service name `mongo` to the MongoDB container IP. Use `mongodb://mongo:27017/app`, not `localhost`.

Express commonly listens on `0.0.0.0:3000` inside its container so other containers can connect. Binding to `127.0.0.1` normally makes it reachable only inside that container.

## ports vs expose
- `ports: ["8080:3000"]` publishes host port 8080 to container port 3000.
- `expose: ["3000"]` documents an internal port; it does not publish it on the host.
- Services on the same user-defined network can communicate without host port publishing.

Docker firewall rules can interact with host firewall policy. Verify actual exposure instead of assuming a host rule alone controls every path.

## Compose teaching example
```yaml
services:
  backend:
    build: ./backend
    environment:
      MONGO_URI: mongodb://mongo:27017/myapp
    depends_on:
      mongo:
        condition: service_healthy
    networks: [private]

  mongo:
    image: mongo:8
    volumes:
      - mongo-data:/data/db
    healthcheck:
      test: ["CMD", "mongosh", "--quiet", "--eval", "db.adminCommand('ping')"]
      interval: 10s
      timeout: 5s
      retries: 5
    networks: [private]

  nginx:
    image: nginx:stable
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./nginx/default.conf:/etc/nginx/conf.d/default.conf:ro
    depends_on: [backend]
    networks: [private]

networks:
  private:
    driver: bridge

volumes:
  mongo-data:
```

This is a teaching example, not a complete production setup. Add database authentication, secrets, backups, TLS automation, health endpoints, resource limits, image pinning, log rotation and monitoring. Verify the chosen Mongo image supports the health-check command.

## Nginx and Express
```nginx
location /api/ {
    proxy_pass http://backend:3000;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
}
```
This is a simplified HTTP proxy location; configure TLS and redirect separately. A trailing slash in `proxy_pass` changes URI rewriting. Configure Express `trust proxy` for only the actual trusted hops, e.g. `app.set('trust proxy', 1)` for exactly one trusted proxy.

## Operations
```bash
docker compose ps
docker compose logs -f nginx
docker compose logs -f backend
docker compose logs -f mongo
docker network ls
docker network inspect <network>
docker compose exec nginx getent hosts backend
docker compose exec nginx wget -qO- http://backend:3000/health
```
`wget` must exist in the Nginx image; otherwise use an installed diagnostic container/client.

## Common failure patterns
- Domain wrong: DNS A/AAAA and caches.
- TCP 443 timeout: route/firewall/listener.
- TLS hostname mismatch: certificate/SNI.
- Nginx 502: upstream connection/process/DNS/port.
- Nginx 504: upstream delay or unresponsive service.
- `ENOTFOUND mongo`: Docker DNS/network membership.
- `ECONNREFUSED mongo:27017`: Mongo process/readiness/listener.
- HTTP 401: authentication.
- HTTP 500: application/dependency error.
