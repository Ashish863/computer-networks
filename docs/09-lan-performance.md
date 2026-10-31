# 9. DHCP, VLANs, MTU, PMTUD and Performance

## DHCP
DHCPv4 commonly uses DORA:
1. Discover (client broadcast)
2. Offer (server offer)
3. Request (client requests)
4. Acknowledge (server confirms)

DHCPv4 uses UDP 67 server and 68 client. A relay forwards requests across subnets. `169.254.0.0/16` may indicate self-assigned link-local IPv4 after DHCP failure. Lease renewal happens before expiry using client timers.

## VLANs
VLANs separate Layer 2 broadcast domains. Access ports normally carry one VLAN; trunks carry multiple VLANs with 802.1Q tags; inter-VLAN traffic requires routing and should be filtered according to policy.

## MTU/MSS
MTU is maximum Layer 3 packet size supported by a link. Standard Ethernet commonly uses IP MTU 1500.
- IPv4/TCP basic MSS: 1500 - 20-byte IPv4 header - 20-byte TCP header = 1460.
- IPv6/TCP basic MSS: 1500 - 40-byte IPv6 base header - 20-byte TCP header = 1440.
Options and extension headers affect actual packet size.

IPv4 routers may fragment packets when permitted. If DF is set and a packet is too large, ICMP fragmentation-needed should be returned. IPv6 routers do not fragment in transit; they send ICMPv6 Packet Too Big. PMTUD discovers usable path size. If needed ICMP is blocked, large transfers can stall while small ones work (PMTU black hole). MSS clamping may be a workaround, not a replacement for correct PMTU behavior.

Windows example:
```powershell
netsh interface ipv4 show subinterfaces
ping 1.1.1.1 -f -l 1472
```
1472 + 8-byte ICMP + 20-byte IPv4 header = 1500. ICMP may be filtered, so failure is not conclusive.

## Performance metrics
- Latency: time for data to travel; RTT is round-trip time.
- Bandwidth: theoretical/configured link capacity.
- Throughput: achieved transfer rate.
- Goodput: useful application payload per time, excluding protocol overhead/retransmitted data.
- Jitter: variation in delay.
- Packet loss: packets fail to arrive.

1 Gbps link capacity with 200 Mbps actual rate means throughput is 200 Mbps. Divide bits/s by 8 for bytes/s before overhead. TCP loss can trigger retransmission and congestion-window reduction, reducing throughput. Loss is not always congestion.

curl timing example:
```powershell
curl.exe -o NUL -s -w "DNS: %{time_namelookup}s`nConnect: %{time_connect}s`nTLS: %{time_appconnect}s`nFirst byte: %{time_starttransfer}s`nTotal: %{time_total}s`n" https://example.com
```
Milestones are cumulative; `time_starttransfer - time_appconnect` is only an approximation of post-TLS time to first byte, not pure application execution time.
