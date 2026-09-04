# Wireshark Network Traffic Analysis

## Overview

This hands-on cybersecurity lab demonstrates how network traffic can be captured, filtered, and analyzed using **Wireshark** inside an isolated Windows 11 virtual machine.

The lab focused on identifying and interpreting common network protocols and connection behavior, including **ICMP**, **DNS**, **TCP**, and **TLS/HTTPS** traffic. I generated controlled network activity, captured the packets, applied Wireshark display filters, inspected packet fields, and documented the findings in a network-analysis report.

## Objectives

- Capture live network traffic from a Windows 11 virtual machine
- Identify the VM's source IP address and communicating hosts
- Analyze ICMP Echo Request and Echo Reply traffic
- Inspect DNS queries and responses for `example.com`
- Identify IPv4 and IPv6 DNS record lookups
- Examine TCP source and destination ports
- Identify a TCP three-way handshake
- Observe TLS 1.3 traffic after TCP connection establishment
- Practice Wireshark display and conversation filters
- Document findings using a structured network-analysis workflow

## Lab Environment

| Component | Configuration |
|---|---|
| Hypervisor | Oracle VirtualBox |
| Guest OS | Windows 11 Pro |
| Hostname | `SOC-LAB-01` |
| VM IP Address | `10.0.2.15` |
| Packet Analyzer | Wireshark 4.6.8 |
| Packet Capture Driver | Npcap |
| Capture Interface | Ethernet |
| Network Mode | VirtualBox NAT |

## 1. Packet Capture

Wireshark was installed inside the Windows 11 lab VM and the active **Ethernet** interface was selected for packet capture.

The unfiltered capture contained a mixture of traffic including TCP, DNS, and TLS 1.3 packets. This provided the baseline dataset for the protocol-specific analysis.

## 2. ICMP Analysis

To generate ICMP traffic, I ran:

```cmd
ping 8.8.8.8
```

The command completed successfully with four replies and 0% packet loss.

I then applied the Wireshark display filter:

```text
icmp
```

The capture showed traffic between:

```text
10.0.2.15  →  8.8.8.8   Echo (ping) request
8.8.8.8    →  10.0.2.15 Echo (ping) reply
```

A detailed ICMP packet inspection showed:

| Field | Observed Value |
|---|---|
| Source IP | `10.0.2.15` |
| Destination IP | `8.8.8.8` |
| Protocol | ICMP |
| Type | `8` — Echo Request |
| Code | `0` |

**ICMP Type 8** represents an Echo Request, while **ICMP Type 0** represents an Echo Reply.

The filtered packet list also contained some ICMP **Time-to-live exceeded** messages from `10.0.2.2`, illustrating that ICMP is used for more than just ping traffic.

## 3. DNS Analysis

To generate DNS traffic, I ran:

```cmd
nslookup example.com
```

The system used DNS server `192.168.1.1` and returned both IPv6 and IPv4 addresses for `example.com`.

In Wireshark, I applied:

```text
dns
```

### DNS Query

One captured DNS query showed:

| Field | Observed Value |
|---|---|
| Source IP | `10.0.2.15` |
| Destination IP | `192.168.1.1` |
| Protocol | UDP / DNS |
| Destination Port | `53` |
| Query Name | `example.com` |
| Record Type | `AAAA` |

An `AAAA` record requests the IPv6 address for a hostname.

### DNS Response

A captured DNS response from `192.168.1.1` to `10.0.2.15` showed a successful response for an `A` record query and returned these IPv4 addresses:

- `104.20.23.154`
- `172.66.147.243`

The response time shown in Wireshark was approximately **4.63 ms**.

An `A` record returns IPv4 addresses, while an `AAAA` record returns IPv6 addresses.

## 4. TCP Three-Way Handshake

To isolate initial TCP connection attempts, I used:

```text
tcp.flags.syn == 1 && tcp.flags.ack == 0
```

I selected a TCP SYN packet from the VM and applied a **Conversation Filter → TCP** to isolate the full connection.

The first three packets in the conversation showed a complete TCP three-way handshake:

```text
1. 10.0.2.15      → 104.46.162.224   55456 → 443   [SYN]
2. 104.46.162.224 → 10.0.2.15        443 → 55456   [SYN, ACK]
3. 10.0.2.15      → 104.46.162.224   55456 → 443   [ACK]
```

This sequence represents:

- **SYN** — the client requests a TCP connection
- **SYN-ACK** — the server acknowledges the request and agrees to establish the connection
- **ACK** — the client acknowledges the server and completes the handshake

The destination port was **443**, indicating HTTPS-related traffic. After the handshake, the same conversation showed **TLS 1.3** packets including a **Client Hello**, demonstrating the transition from TCP connection establishment to encrypted application-layer communication.

> Note: this captured TCP conversation was valid HTTPS traffic observed in the lab environment and was used to demonstrate the handshake. It was not attributed to the `example.com` lookup because the observed destination IP did not match the DNS answers captured for `example.com`.

## Key Findings

- The Windows VM used `10.0.2.15` as its IPv4 address inside the VirtualBox NAT environment.
- ICMP traffic clearly showed Echo Request and Echo Reply behavior between the VM and `8.8.8.8`.
- DNS traffic showed the VM querying `192.168.1.1` on UDP port 53 for `example.com`.
- The DNS analysis demonstrated both `A` and `AAAA` record concepts.
- A DNS response returned IPv4 addresses `104.20.23.154` and `172.66.147.243`.
- A captured HTTPS TCP conversation demonstrated the complete SYN → SYN-ACK → ACK handshake.
- TLS 1.3 traffic followed the completed TCP connection, including a Client Hello.

## Wireshark Filters Used

```text
icmp
dns
tcp.flags.syn == 1
tcp.flags.syn == 1 && tcp.flags.ack == 0
ip.addr == <IP> && tcp
tcp.stream eq <stream-number>
```

## Skills Demonstrated

- Wireshark packet capture
- Network traffic analysis
- Packet filtering
- ICMP analysis
- DNS analysis
- TCP/IP fundamentals
- TCP three-way handshake analysis
- Source and destination IP analysis
- TCP/UDP port interpretation
- TLS/HTTPS traffic recognition
- IPv4 and IPv6 DNS record analysis
- Network troubleshooting fundamentals
- SOC/NOC investigation methodology

## Repository Structure

```text
Wireshark-Network-Traffic-Analysis/
├── README.md
├── investigation/
│   └── network-analysis-report.md
└── screenshots/
    └── README.md
```

## Network Analysis Report

A more formal write-up of the observed traffic is available here:

[View the network analysis report](investigation/network-analysis-report.md)

## Future Improvements

Future versions of this lab can be expanded with:

- HTTP request analysis
- TLS handshake analysis in greater detail
- TCP retransmission analysis
- ARP traffic analysis
- DHCP traffic analysis
- Suspicious DNS query detection
- Port scan detection
- PCAP analysis from known security incidents
- Splunk or Microsoft Sentinel ingestion of network telemetry
