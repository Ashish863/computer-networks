# 7. NAT, IPv6, Firewalls and Security

## NAT/PAT
NAT rewrites address information across a boundary. PAT multiplexes internal flows through a shared public IPv4 address using transport ports and state. NAT is not inherently a firewall, though devices commonly combine translation and filtering.

Port publishing example: Docker `-p 8080:3000` maps host port 8080 to container port 3000. Reachability still depends on bind behavior, routing and firewall policy. CGNAT may prevent unsolicited inbound connections because the ISP controls the translation.

## IPv6
IPv6 is 128-bit, written in hexadecimal groups. `::1` is loopback; `fe80::/10` link-local; `fc00::/7` unique-local; `ff00::/8` multicast. IPv6 has no broadcast. NDP (ICMPv6) handles neighbor discovery; SLAAC can configure addresses from router advertisements; DHCPv6 may also be used. IPv6 does not remove the need for firewalls.

## Firewalls
A firewall filters traffic by properties such as source/destination, protocol, port, interface and connection state. It may be stateless or stateful.

Distinguish:
- Listening: process bound a socket.
- Reachable: routing/filtering allows packets to reach it.
- Authenticated: identity/credential proven.
- Authorized: identity permitted to perform action.

Allowing TCP 443 does not make the API route healthy or authorized.

## Least privilege
Typical single-VPS policy:
- Public: HTTPS 443; HTTP 80 for redirect/certificate workflows.
- SSH: restricted to trusted management sources where practical.
- Express: internal only.
- MongoDB: internal only.
Use cloud firewall, host firewall, container network design, application auth, database credentials, secret management, updates and backups. VLANs and Docker bridges are not complete security boundaries by themselves.

## Threats
Port scanning discovers exposed services; DoS/DDoS exhaust resources; MITM intercepts or modifies traffic; DNS spoofing can return false data; lateral movement uses one foothold to reach internal systems.

## Review
1. Does NAT automatically provide a complete security policy?
2. Listening vs publicly reachable?
3. Why firewall IPv6?
