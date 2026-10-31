# 1. Network Foundations, Models and Encapsulation

## What is a network?
A network is a collection of endpoints and forwarding devices connected by links and governed by protocols. Endpoints include laptops, phones, servers, VMs and containers. Forwarders include switches, routers, firewalls and load balancers. A protocol defines message formats, state transitions, timing and error handling.

LAN is a local network; WAN spans larger geography; WLAN is wireless LAN; the Internet is an interconnection of independently operated IP networks. A LAN can work without Internet access.

## OSI and TCP/IP models
| OSI | Role | Examples |
|---|---|---|
| 7 Application | Application semantics | HTTP, DNS, SMTP |
| 6 Presentation | Representation/encryption concepts | Encoding, TLS concepts |
| 5 Session | Dialog/session concepts | Session coordination |
| 4 Transport | Process-to-process delivery | TCP, UDP |
| 3 Network | Logical addressing/routing | IPv4, IPv6, ICMP |
| 2 Data link | Local-link framing | Ethernet, Wi-Fi |
| 1 Physical | Signals/media | Copper, fiber, radio |

The practical TCP/IP model is commonly Application, Transport, Internet, Link (sometimes Link is split into data-link and physical). Models are diagnostic abstractions, not seven separate boxes in every implementation.

## Encapsulation
Application data is wrapped by lower-layer protocol metadata:

```text
Application data
  -> TCP segment (TCP header)
  -> IP packet (IP header)
  -> Ethernet frame (Ethernet header + FCS trailer)
  -> signals/bits on the medium
```

The receiver decapsulates in reverse. Common names: Ethernet frame, IP packet, TCP segment, UDP datagram. A PDU is a protocol data unit.

## Identifiers
- MAC address: local-link interface delivery.
- IP address: logical network endpoint and routing.
- Port: transport endpoint on a host.
- Application identity: route, user, tenant or resource.

A router normally replaces the incoming link-layer header for the outgoing link. IP source/destination usually remain stable across routing, except when NAT or other transformations apply. TTL/hop-limit changes at each routed hop.

## Practical commands
Windows: `ipconfig /all`, `route print`, `arp -a`.
Linux: `ip address`, `ip route`, `ip neigh`.

## Misconceptions
- Ping success does not prove TCP 443 works.
- An IP address alone does not identify a process.
- MAC addresses are not end-to-end Internet routing identifiers.

## Review
1. Which layer forwards using IP prefixes?
2. Why does a router create a new Layer 2 frame?
3. What identifies a TCP endpoint?
