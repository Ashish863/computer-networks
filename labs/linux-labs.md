# Linux Networking Labs

## 1. Interfaces
`ip -br link`
`ip -br address`

## 2. Routing
`ip route`
`ip route get 1.1.1.1`

## 3. Neighbors
`ip neigh`

## 4. DNS
`dig example.com A`
`dig example.com AAAA`
`dig +trace example.com`
`getent ahosts example.com`

## 5. Sockets
`sudo ss -lntup`
`ss -tn`
Understand listening vs established sockets.

## 6. HTTP/TLS
`curl -v https://example.com`
`openssl s_client -connect example.com:443 -servername example.com`

## 7. Capture
`sudo tcpdump -ni any 'tcp port 443'`
`sudo tcpdump -ni any 'udp port 53'`
`sudo tcpdump -ni any -w capture.pcap 'tcp port 443'`
`tcpdump -nn -r capture.pcap`
Capture only authorized traffic; PCAPs can contain secrets.

## 8. Docker DNS
```bash
docker network create lab-net
docker run -d --name web-lab --network lab-net nginx
docker run --rm --network lab-net busybox nslookup web-lab
docker rm -f web-lab
docker network rm lab-net
```
Observe container-name resolution on a user-defined bridge.
