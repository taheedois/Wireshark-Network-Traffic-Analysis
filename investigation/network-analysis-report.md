# Network Analysis Report — Wireshark Traffic Investigation

## Summary

A Windows 11 virtual machine named `SOC-LAB-01` was used to generate and capture controlled network traffic with Wireshark. The analysis focused on ICMP, DNS, TCP, and TLS/HTTPS behavior.

## Scope

- Host: `SOC-LAB-01`
- VM IPv4 Address: `10.0.2.15`
- Capture Interface: Ethernet
- Network Mode: VirtualBox NAT
- Packet Analyzer: Wireshark
- Protocols Reviewed: ICMP, DNS, TCP, TLS

## Traffic Generation

The following actions were used to generate network traffic:

```cmd
ping 8.8.8.8
nslookup example.com
```

Additional HTTPS/TCP traffic was observed during the capture and used to examine connection establishment behavior.

## ICMP Findings

The `ping 8.8.8.8` command returned four successful replies with 0% packet loss.

Wireshark showed ICMP Echo Request traffic from:

```text
10.0.2.15 → 8.8.8.8
```

and corresponding Echo Reply traffic from:

```text
8.8.8.8 → 10.0.2.15
```

A detailed packet showed ICMP Type `8`, Code `0`, which identifies an Echo Request. Echo Replies use ICMP Type `0`.

The capture also included ICMP Time-to-live exceeded messages from `10.0.2.2`, demonstrating another use of ICMP for network control and diagnostics.

## DNS Findings

The lab system queried DNS server `192.168.1.1` for `example.com` using UDP port `53`.

One query requested an `AAAA` record, indicating an IPv6 address lookup.

A captured `A` record response returned:

- `104.20.23.154`
- `172.66.147.243`

The DNS response indicated no error and showed a response time of approximately `4.63 ms`.

This traffic demonstrated the distinction between:

- `A` records — IPv4 addresses
- `AAAA` records — IPv6 addresses

## TCP Findings

An HTTPS-related TCP conversation was isolated using Wireshark's TCP conversation filter.

The first three packets showed:

```text
10.0.2.15      → 104.46.162.224   55456 → 443   [SYN]
104.46.162.224 → 10.0.2.15        443 → 55456   [SYN, ACK]
10.0.2.15      → 104.46.162.224   55456 → 443   [ACK]
```

This confirms a complete TCP three-way handshake.

The conversation then continued with TLS 1.3 traffic, including a Client Hello, showing that encrypted HTTPS communication began after TCP connection establishment.

The destination IP `104.46.162.224` did not match the DNS answers captured for `example.com`, so the TCP conversation was treated as separate HTTPS traffic rather than attributed to the `example.com` lookup.

## Analysis

The capture demonstrates how a single workstation can generate several protocol types at the same time. Wireshark display filters made it possible to isolate each traffic type and inspect protocol-specific fields.

ICMP analysis showed basic reachability testing, DNS analysis showed hostname-to-IP resolution, and TCP analysis demonstrated how reliable connections are established before encrypted TLS traffic begins.

## Conclusion

The lab successfully captured and analyzed common network traffic from a Windows 11 virtual machine. The investigation demonstrated practical understanding of ICMP request/reply behavior, DNS resolution, TCP connection establishment, port usage, and TLS/HTTPS traffic recognition.

These skills are directly relevant to entry-level SOC, NOC, cybersecurity analyst, and network troubleshooting work.

## Analyst Notes

This traffic was intentionally generated and analyzed in an isolated lab environment for defensive cybersecurity training and portfolio development.
