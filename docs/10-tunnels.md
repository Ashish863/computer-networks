# 10. VPNs, Proxies and Tunnels

A VPN creates a logical network path over another network, commonly with encryption and authentication. Remote-access VPNs connect users to private resources; site-to-site VPNs connect networks. Tunnels encapsulate inner packets in outer packets. Added headers reduce effective payload MTU and can trigger PMTU issues.

Forward proxy: acts for clients sending requests to external destinations. Reverse proxy: acts for servers, accepting external requests and forwarding to internal services. Nginx in front of Express is a reverse proxy.

SSH local forwarding:
```bash
ssh -L 27018:127.0.0.1:27017 user@your-server
```
A local listener on port 27018 sends traffic through SSH; the SSH server side connects to its own `127.0.0.1:27017`. This can provide controlled database administration without publishing MongoDB publicly. Use keys, restrict SSH and close the tunnel when finished.

If VPN traffic works for small payloads but large transfers stall, investigate tunnel overhead, MTU/PMTUD, routes, DNS and access policy.
