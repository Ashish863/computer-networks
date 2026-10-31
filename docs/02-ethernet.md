# 2. Ethernet, MAC, ARP and Switching

## Ethernet frame
Ethernet II has destination MAC (6 bytes), source MAC (6), EtherType (2), payload and FCS (4). A VLAN tag adds 4 bytes. Standard Ethernet commonly carries an IP MTU of 1500 bytes; link-layer headers are not included in that IP MTU convention.

MAC addresses are usually 48-bit values shown as six hexadecimal octets. Unicast identifies one interface; multicast identifies a group; Ethernet broadcast is `FF:FF:FF:FF:FF:FF`.

## Switch learning
A switch learns the source MAC and ingress port/VLAN into its forwarding database (FDB). It looks up the destination:
- Known unicast: forward to the associated port.
- Unknown unicast: flood within that VLAN, excluding ingress.
- Broadcast: replicate within the broadcast domain.
Entries age out if not refreshed. A switch learns from source MACs.

## ARP
ARP resolves an IPv4 next-hop address to a MAC on the local link. If destination is on-link, resolve the destination's MAC. If destination is remote, resolve the default gateway's MAC—not the remote server's MAC. The IP destination remains the remote server while the Ethernet destination is the gateway.

IPv6 uses Neighbor Discovery Protocol (NDP) over ICMPv6 rather than ARP.

Commands:
- Windows: `arp -a`, `Get-NetNeighbor`
- Linux: `ip neigh`
- Wireshark: `arp`

## VLANs
A VLAN separates Layer 2 broadcast domains. Access ports normally carry one VLAN; trunk ports carry multiple VLANs using 802.1Q tags. Inter-VLAN communication requires Layer 3 routing. VLANs are segmentation, not encryption or a complete security boundary.

## Review
1. For off-subnet traffic, which MAC is resolved?
2. What does a switch do with unknown unicast?
3. What does a VLAN isolate?
