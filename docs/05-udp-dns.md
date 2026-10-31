# 5. UDP, QUIC and DNS

## UDP and QUIC
UDP is connectionless and datagram-oriented. It provides ports and checksum, but not TCP-style ordering, retransmission or built-in congestion control. Applications may implement these features above UDP.

TCP is an ordered byte stream; UDP preserves datagram boundaries. QUIC uses UDP as substrate but implements encrypted reliable streams, congestion control and connection management. HTTP/3 runs over QUIC.

## DNS hierarchy
DNS is a distributed naming system. A stub resolver asks a recursive resolver. The recursive resolver checks cache, then may query root, TLD and authoritative servers. The authoritative server publishes records for its zone; the recursive resolver returns and caches the answer according to TTL.

Common records:
- A: IPv4
- AAAA: IPv6
- CNAME: alias
- MX: mail exchanger
- NS: nameserver delegation
- TXT: text/verification/policy
- PTR: reverse lookup
- SOA: zone metadata
- SRV: service location

TTL controls how long a record may be cached; changes are not instantly visible in all caches. Registrar, registry, DNS host and application hosting are distinct roles.

DNSSEC authenticates DNS data using signatures; it does not encrypt queries or certify that a website is safe. DoH and DoT encrypt client-to-resolver transport, but the resolver still sees the query.

## Commands
Windows: `nslookup example.com`, `ipconfig /displaydns`, `ipconfig /flushdns`.
Linux: `dig example.com A`, `dig example.com AAAA`, `dig +trace example.com`, `getent ahosts example.com`.

## Docker DNS
Within a user-defined Docker bridge/Compose network, service names can resolve to container IPs. Use `mongodb://mongo:27017/app` from another service, not `localhost`: localhost inside a container is that container itself.

## Review
1. Recursive vs authoritative DNS?
2. Does DNSSEC encrypt queries?
3. Why might a DNS change be delayed?
