# Computer Networks Interview Preparation

## Core questions
1. **What happens when a URL is entered?** URL parsing/cache, DNS, route selection, ARP/NDP for next hop, TCP or QUIC, TLS, HTTP, proxy/load balancer, application, dependencies and rendering. Reuse/caching can skip some setup.
2. **MAC vs IP?** MAC is local-link delivery; IP is logical addressing/routing. Routers use new Layer 2 frames per hop.
3. **TCP vs UDP?** TCP is reliable ordered byte stream; UDP is datagram-oriented without TCP-style guarantees.
4. **TCP handshake?** SYN, SYN-ACK, ACK; synchronizes sequence numbers and connection state.
5. **Flow vs congestion control?** `rwnd` protects receiver; `cwnd` protects network.
6. **DNS TTL?** Maximum cache lifetime for a DNS answer.
7. **NAT?** Translation of packet addresses, often with PAT multiplexing ports.
8. **Reverse proxy?** Server-side proxy accepting client requests and forwarding to backends.
9. **CORS?** Browser-enforced cross-origin access policy, not authentication.
10. **MTU?** Maximum Layer 3 packet size supported by a link.

## Scenario questions
- Domain resolves but HTTPS times out: inspect TCP 443, route, cloud/host firewall and listener.
- TCP works but certificate fails: inspect hostname, chain, expiry, SNI and clock.
- Nginx 502: inspect upstream address, backend process, Docker DNS, listener and port.
- Nginx 504: inspect backend latency, database calls, saturation, network delay and timeout configuration.
- Express cannot reach Mongo: verify URI, service name, shared network, readiness, port and credentials.
- Large upload stalls: inspect PMTUD/MTU, tunnel overhead, packet loss, proxy limits/timeouts, buffering and application streaming.

## Advanced prompts
- Why is TCP a byte stream?
- How does SACK help loss recovery?
- Why can HTTP/2 still experience TCP head-of-line blocking?
- Why does PMTUD depend on ICMP?
- Why is listening not the same as reachable?
- How do Docker namespaces and service DNS work?
- Why is trusting arbitrary `X-Forwarded-For` unsafe?
- Why does ping not prove an HTTPS application is healthy?

## Answer structure
State the layer, explain what the symptom proves and does not prove, name the next diagnostic command/log, mention an alternative cause, and propose a safe corrective action.
