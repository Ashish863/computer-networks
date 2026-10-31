# 4. TCP Deep Dive

## TCP service
TCP provides a reliable, ordered byte stream. It does not preserve application write/message boundaries: three writes may be read in different groupings. Applications need framing (length prefix, delimiter, etc.) when message boundaries matter.

TCP includes connection state, sequencing, acknowledgements, retransmission, flow control and congestion control. A TCP ACK does not mean the application committed a database transaction; it acknowledges transport-layer receipt.

## Connection tuple and handshake
A TCP flow is identified by source IP/port, destination IP/port and protocol.

```text
Client                         Server
  | -------- SYN, seq=x -------> |
  | <--- SYN-ACK, seq=y, ack=x+1 |
  | -------- ACK, ack=y+1 ------>|
```

The handshake synchronizes initial sequence numbers and negotiates options such as MSS, window scaling, SACK and timestamps. A listening socket differs from an established connection.

## Sequence and ACK arithmetic
Sequence numbers count bytes. If a segment starts at 5000 and carries 500 bytes, next cumulative ACK is 5500. SYN and FIN each consume one sequence number. ACK generally means "next byte expected." SACK reports non-contiguous received blocks to improve recovery.

## Flow control vs congestion control
- `rwnd`: receiver-advertised receive window; protects receiver buffers.
- `cwnd`: sender congestion window; limits injection into the network based on congestion signals.
Simplified in-flight bound: `in-flight data <= min(rwnd, cwnd)`.

Zero-window advertisement means receiver currently cannot accept more data; TCP uses persist probing to detect window updates.

## Loss and congestion
TCP detects loss using duplicate ACK/SACK patterns and retransmission timeout (RTO). Fast retransmit can recover before RTO. Retransmissions consume capacity and add delay. Congestion control may reduce `cwnd`, lowering throughput. Loss is not always congestion: wireless interference, faulty links and routing changes can also cause loss.

Algorithms vary (Reno, CUBIC, BBR). Slow start grows rapidly; congestion avoidance grows more cautiously; congestion signals trigger reduction/recovery behavior.

## Teardown
FIN closes one direction gracefully; TCP is full duplex, so each direction can close independently. RST aborts or rejects a connection.

Commands:
- Windows: `netstat -ano -p tcp`, `Get-Process -Id <PID>`
- Linux: `ss -lntp`, `ss -tn`, `ss -ti`
Wireshark filters: `tcp`, `tcp.flags.syn == 1`, `tcp.flags.reset == 1`, `tcp.analysis.retransmission`, `tcp.analysis.duplicate_ack`.

## Review
1. Seq 12000 plus 800 bytes gives what next ACK?
2. Difference between `rwnd` and `cwnd`?
3. Why can packet loss reduce TCP throughput?
