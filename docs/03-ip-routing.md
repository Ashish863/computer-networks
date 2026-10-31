# 3. IPv4, CIDR, Subnetting and Routing

## IPv4 and prefixes
IPv4 is 32 bits, commonly written as four decimal octets. `192.168.10.25/24` means the first 24 bits are the network prefix; `/24` mask is `255.255.255.0`.

Private ranges (RFC 1918):
- `10.0.0.0/8`
- `172.16.0.0/12`
- `192.168.0.0/16`

Other useful ranges: `127.0.0.0/8` loopback; `169.254.0.0/16` link-local (often seen after DHCP failure). `0.0.0.0` is context-dependent unspecified/wildcard, not a normal remote destination.

## Subnet arithmetic
For prefix `/p`, host bits = `32-p`, total addresses = `2^(32-p)`. Traditional IPv4 usable host count is total minus network and broadcast addresses, with exceptions such as `/31` point-to-point and `/32` host routes.

| Prefix | Total | Traditional usable |
|---|---:|---:|
| /24 | 256 | 254 |
| /25 | 128 | 126 |
| /26 | 64 | 62 |
| /27 | 32 | 30 |
| /28 | 16 | 14 |
| /30 | 4 | 2 |

Example `192.168.1.64/26`: network `.64`, hosts `.65`–`.126`, broadcast `.127`. A `/26` has 64 total addresses, not 64 traditional usable hosts.

## Routing
A route includes destination prefix, next hop, output interface and metric/preference. Routers use longest-prefix match: the most-specific matching prefix wins. Default route is `0.0.0.0/0` (IPv4) or `::/0` (IPv6), used only when no more-specific route matches.

For a remote destination, a host sends the first Ethernet frame to its default gateway's MAC. The router forwards based on destination IP, decrements TTL and creates a new link-layer frame for the next link.

Commands:
- Windows: `route print`, `tracert 1.1.1.1`
- Linux: `ip route`, `ip route get 1.1.1.1`, `traceroute 1.1.1.1`

Traceroute probes can be filtered; `*` does not automatically mean path failure.

## Review
1. How many traditional usable addresses in /26?
2. Define longest-prefix match.
3. What is the first-hop Ethernet destination for an off-subnet packet?
