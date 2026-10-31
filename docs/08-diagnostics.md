# 8. Packet Capture and Troubleshooting

## Troubleshooting discipline
Treat troubleshooting as hypothesis testing. Define source, destination, protocol, port, time and exact symptom. Test one boundary at a time:

1. DNS
2. Routing/reachability
3. TCP
4. TLS
5. HTTP/proxy
6. Application
7. Dependencies/database

DNS success does not prove server reachability. Ping success does not prove a TCP service works.

## Refused vs timeout
Connection refused often means RST or no listener/active rejection and usually fails quickly. Timeout means no successful connection before deadline; possible causes include DROP filtering, route failure, host outage or severe loss. Neither uniquely identifies root cause.

## Windows toolkit
```powershell
ipconfig /all
ipconfig /displaydns
ipconfig /flushdns
nslookup example.com
route print
arp -a
tracert example.com
Test-NetConnection example.com -Port 443
netstat -ano
curl.exe -v https://example.com
```

## Linux toolkit
```bash
ip -br address
ip route
ip route get 1.1.1.1
ip neigh
ss -lntup
ss -tn
dig example.com
getent ahosts example.com
curl -v https://example.com
traceroute example.com
sudo tcpdump -ni any 'host 203.0.113.10 and tcp port 443'
```

## Packet interpretation
- SYN: connection attempt.
- SYN-ACK: server responded to SYN.
- ACK: handshake completion.
- RST: reset/rejection.
- Repeated SYN without response: filtering, route failure or unavailable endpoint.
- Retransmission: sender did not receive expected acknowledgement in time; investigate both directions.

Wireshark filters:
`dns`, `tcp`, `tcp.flags.syn == 1`, `tcp.flags.reset == 1`, `tcp.analysis.retransmission`, `tls`.

TLS 1.3 encrypts much of the handshake after ServerHello, so packet visibility differs from older TLS versions.

## Incident checklist
Confirm scope/time; inspect DNS A/AAAA; provider and VPS health; cloud and host firewall; listening sockets; Nginx logs; container status/restarts; backend logs/request ID; database reachability/auth/pool; CPU/memory/disk/file descriptors; TLS chain/expiry/hostname; recent changes.

Never disable firewalls as the first step, expose databases for testing, capture unauthorized traffic, or paste secrets/tokens into logs.
