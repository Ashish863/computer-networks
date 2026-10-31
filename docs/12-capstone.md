# 12. End-to-End Troubleshooting Capstone

## Scenario
A React frontend calls `https://myapp.example/api/users`. Nginx is the public ingress, Express runs in Docker, and MongoDB is on a private Compose network. Users report intermittent failures.

## Diagnose from outside inward
1. DNS: `nslookup myapp.example` or `dig myapp.example A`. Verify A and AAAA records.
2. Route/reachability: `tracert` or `traceroute`; remember intermediate ICMP may be filtered.
3. TCP: Windows `Test-NetConnection myapp.example -Port 443`; Linux `nc -vz myapp.example 443` if installed.
4. TLS: `curl -v https://myapp.example`; Linux `openssl s_client -connect myapp.example:443 -servername myapp.example`.
5. HTTP: inspect status, response headers, redirects and timing.
6. Nginx: inspect access/error logs and upstream address.
7. Docker: `docker compose ps`, `docker network inspect`, resolve `backend` from Nginx.
8. Express: inspect health endpoint and logs with request/correlation ID.
9. MongoDB: inspect health, logs, service DNS, port, credentials and connection pool.

## Scenario questions
1. DNS resolves correctly, TCP 443 fails. What do you inspect?
2. TCP works but certificate hostname mismatches. Which layer?
3. Nginx reports `connect() failed (111: Connection refused)` to `backend:3000`. What does this suggest?
4. `mongo` resolves but port 27017 refuses. What next?
5. API returns 401. Is this a routing failure?
6. Small responses work, large upload stalls. What network and application factors could contribute?

## Model answers
1. Listener, route, cloud firewall and host firewall; DNS has already succeeded.
2. TLS certificate/SNI/hostname configuration.
3. Backend process, bind address, port, container health and network path.
4. MongoDB readiness/process/listener, port, logs and authentication.
5. Usually application authentication; inspect API auth behavior.
6. MTU/PMTUD, tunnel overhead, packet loss, proxy body limits/timeouts, buffering and application streaming.

## Final incident checklist
- Record exact URL, source, time and scope.
- Check DNS A/AAAA.
- Check provider/VPS health.
- Check cloud and host firewalls.
- Check listening sockets.
- Check Nginx logs.
- Check container health and restart count.
- Check backend logs and request ID.
- Check database connectivity and pool.
- Check CPU, memory, disk and file descriptors.
- Check TLS chain, expiry and hostname.
- Review recent deployments/config changes.
- Document root cause, mitigation and prevention.
