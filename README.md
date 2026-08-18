# Wireshark Network Traffic Analysis Lab

## Project Overview

This project documents a hands-on network traffic investigation using Wireshark. A packet capture (PCAPNG) was collected from a Windows workstation and analysed to identify DNS activity, encrypted TLS connections, and outbound IPv4 conversations.

The lab demonstrates foundational Security Operations Centre (SOC) and digital-forensics skills: capturing packets, applying display filters, correlating network metadata, and documenting findings.

## Tools Used

- Wireshark
- Npcap
- Windows 11

## Capture Details

- Capture format: `.pcapng`
- Capture interface: Wi-Fi
- Local device: `192.168.0.6`
- Local gateway / DNS resolver: `192.168.0.1`

> Private IP addresses are included only to describe the isolated home-lab capture.

## Investigation Tests

### 1. DNS Traffic Analysis

**Objective:** Identify domains requested by the local device and correlate them with returned IP addresses.

**Wireshark display filter:**

```text
dns
```

**Findings:**

- Observed DNS queries from `192.168.0.6` to `192.168.0.1`.
- Identified queries for domains including `chatgpt.com`, `android.clients.google.com`, and `ssl.gstatic.com`.
- Confirmed DNS response packets returning associated IPv4 addresses.
- Verified that DNS traffic used UDP port 53.

![DNS traffic analysis](screenshots/dns-analysis.png)

### 2. TLS / SNI Analysis

**Objective:** Investigate encrypted web connections and identify the requested service using TLS handshake metadata.

**Wireshark display filter:**

```text
tls
```

**Findings:**

- Identified a TLS 1.3 Client Hello from the local device to a remote server over TCP port 443.
- Reviewed the `server_name` extension in the Client Hello.
- Confirmed the Server Name Indication (SNI) value as `chatgpt.com`.
- Demonstrated that TLS encrypts the web-session content while selected connection metadata can remain observable.

![TLS SNI analysis](screenshots/tls-sni-analysis.png)

### 3. IPv4 Conversation Review

**Objective:** Review outbound communications and correlate external IP connections with earlier DNS and TLS evidence.

**Wireshark location:**

```text
Statistics → Conversations → IPv4
```

**Findings:**

- Reviewed IPv4 conversations involving the local device.
- Observed communications with the local router and multiple external IP addresses.
- Correlated expected external services with DNS query and TLS SNI evidence.
- Recorded unfamiliar external connections as items that would require further investigation in a SOC workflow.

![IPv4 conversations](screenshots/ipv4-conversations.png)

## Key Skills Demonstrated

- Packet capture and PCAP analysis
- Wireshark display filters
- DNS investigation
- TLS handshake and SNI analysis
- IP address and port analysis
- Network traffic correlation
- Security-event documentation

## Project Structure

```text
.
├── wireshark-network-analysis.pcapng
├── README.md
└── screenshots/
    ├── dns-analysis.png
    ├── tls-sni-analysis.png
    └── ipv4-conversations.png
```

## Notes

This capture is for learning and portfolio purposes. No attempt was made to decrypt encrypted TLS application data; the analysis focused on observable network metadata.
