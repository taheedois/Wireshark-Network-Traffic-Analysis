# Screenshot Evidence

This folder contains the packet-analysis evidence used in the Lab 2 README and network-analysis report.

1. `01-wireshark-capture.png`
   - General Wireshark packet capture overview
   - Shows mixed TCP, TLS, and DNS traffic on the Ethernet interface

2. `02-dns-query-details.png`
   - DNS query for `example.com`
   - Shows source `10.0.2.15`, destination `192.168.1.1`, UDP destination port `53`, and an `AAAA` lookup

3. `03-icmp-analysis.png`
   - Filtered ICMP traffic
   - Shows Echo Request and Echo Reply traffic between `10.0.2.15` and `8.8.8.8`

4. `04-icmp-packet-details.png`
   - Detailed ICMP Echo Request packet fields
   - Shows ICMP Type `8` and Code `0`

5. `05-dns-response-details.png`
   - DNS response for an `A` record query for `example.com`
   - Shows returned IPv4 addresses `104.20.23.154` and `172.66.147.243`

6. `06-tcp-three-way-handshake.png`
   - Isolated TCP conversation
   - Shows `[SYN]`, `[SYN, ACK]`, and `[ACK]` packets followed by TLS 1.3 traffic

## Notes

The screenshots were captured in an isolated Windows 11 VirtualBox lab for defensive cybersecurity training and portfolio documentation.
