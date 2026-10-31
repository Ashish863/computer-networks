# Windows Networking Labs

Use PowerShell on your own device or an authorized network.

## 1. Interface details
`ipconfig /all`
Record interface, IPv4/IPv6, mask, gateway, DNS and DHCP status.

## 2. DNS
`nslookup example.com`
`nslookup -type=MX example.com`
`ipconfig /displaydns`
Compare record types and resolver answers.

## 3. Routes and neighbors
`route print`
`arp -a`
`tracert 1.1.1.1`
Find default route and gateway. Traceroute hops may not reply.

## 4. TCP service
`Test-NetConnection example.com -Port 443`
Compare with port 22. Failure on 22 does not mean host is down.

## 5. Local sockets
`netstat -ano`
Find PID, then `Get-Process -Id <PID>`. Do not terminate unknown processes.

## 6. HTTP/TLS
`curl.exe -v https://example.com`
Use `curl.exe -o NUL -s -w "DNS %{time_namelookup}s Connect %{time_connect}s TLS %{time_appconnect}s FirstByte %{time_starttransfer}s Total %{time_total}s\n" https://example.com`.
Milestones are cumulative, not isolated server timings.

## 7. MTU probe
`netsh interface ipv4 show subinterfaces`
`ping 1.1.1.1 -f -l 1472`
1472 + ICMP 8 + IPv4 20 = 1500. ICMP filtering makes failure inconclusive.

## Lab report
Objective, environment, hypothesis, command, expected observation, actual observation, what it proves, alternative explanations, conclusion.
