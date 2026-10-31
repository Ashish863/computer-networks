# Wireshark and Packet Capture Labs

Capture only traffic you own or are authorized to inspect. Protect capture files: they may contain sensitive metadata or credentials.

## TCP handshake
Display filter: `tcp.flags.syn == 1`. Find SYN, SYN-ACK, ACK. Record IPs, ports, sequence numbers and negotiated options. SYN alone does not prove connection establishment.

## DNS
Filter: `dns`. Inspect query name/type, transaction ID, response code, answer and TTL.

## Retransmission
Filter: `tcp.analysis.retransmission`. Inspect timing and sequence ranges. Wireshark labels are heuristic; validate packet sequence and capture location.

## TLS
Filter: `tls`. Inspect ClientHello, SNI (when visible), ServerHello and alerts. TLS 1.3 encrypts much of the handshake after ServerHello.

## HTTP
Filter: `http` for cleartext test traffic. HTTPS application content is encrypted unless authorized decryption is configured.

## tcpdump
`sudo tcpdump -ni eth0 'tcp port 443'`
`sudo tcpdump -ni any 'host 203.0.113.10'`
`sudo tcpdump -ni any -w capture.pcap 'tcp port 443'`
