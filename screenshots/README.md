# Screenshot Evidence

Upload the following Lab 2 screenshots to this folder using these filenames:

1. `01-wireshark-capture.png`
   - General Wireshark packet capture overview
   - Shows mixed TCP, TLS, and DNS traffic on the Ethernet interface

2. `02-dns-query-details.png`
   - Shows the DNS query for `example.com`
   - Includes source `10.0.2.15`, destination `192.168.1.1`, UDP destination port `53`, and an `AAAA` lookup

3. `03-icmp-analysis.png`
   - Shows filtered ICMP traffic
   - Includes Echo Request and Echo Reply traffic between `10.0.2.15` and `8.8.8.8`

4. `04-icmp-packet-details.png`
   - Shows detailed ICMP Echo Request packet fields
   - Includes ICMP Type `8` and Code `0`

5. `05-dns-response-details.png`
   - Shows the DNS response for an `A` record query for `example.com`
   - Includes returned IPv4 addresses `104.20.23.154` and `172.66.147.243`

6. `06-tcp-three-way-handshake.png`
   - Shows the isolated TCP conversation
   - Includes `[SYN]`, `[SYN, ACK]`, and `[ACK]` packets
   - Demonstrates connection establishment before TLS 1.3 traffic

## Screenshot Safety

Before uploading screenshots, review them for unrelated personal information, browser tabs, personal email addresses, host-PC usernames, or other data not needed to demonstrate the lab findings. Crop or redact anything unnecessary.
