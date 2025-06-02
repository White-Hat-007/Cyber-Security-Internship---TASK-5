# Wireshark Traffic Analysis – 60s Google Communication

This document analyzes communication protocols between a local machine and Google servers using Wireshark.

---

## 📡 Interface Used

- **Interface**: `eth0`

---

## 🕒 Capture Overview

- **Duration**: 60 seconds
- **Target**: `www.google.com`
- **Activities**: DNS resolution, ICMP (ping), and passive observation of ARP interactions.

---

## 🔍 Protocols Captured

---

### 1. 🧭 Address Resolution Protocol (ARP)

**Purpose**: Resolves IP addresses to MAC addresses on a local network.

#### 📦 Sample Packet Breakdown

- **Frame Length**: 60 bytes
- **Protocol Type**: ARP (0x0806)
- **Hardware Type**: Ethernet (1)
- **Protocol Type**: IPv4 (0x0800)
- **Hardware Size**: 6
- **Protocol Size**: 4
- **Opcode**: Request (1) or Reply (2)
- **Sender MAC Address**: `AA:BB:CC:DD:EE:FF`
- **Sender IP Address**: `192.168.0.5`
- **Target MAC Address**: `00:00:00:00:00:00` (in Request)
- **Target IP Address**: `192.168.0.1`

#### 📝 Observations

- ARP Request: "Who has 192.168.0.1? Tell 192.168.0.5"
- ARP Reply: "192.168.0.1 is at 00:11:22:33:44:55"

---

### 2. 🌐 Domain Name System (DNS)

**Purpose**: Translates human-readable domain names into IP addresses.

#### 📦 Sample DNS Query Packet

- **Frame Length**: ~74 bytes
- **Protocol**: DNS over UDP (Port 53)
- **Transaction ID**: 0x1a2b
- **Flags**: Standard query (0x0100)
- **Questions**: 1
- **Answer RRs**: 0
- **Authority RRs**: 0
- **Additional RRs**: 0
- **Query Name**: `www.google.com`
- **Query Type**: A (host address)
- **Query Class**: IN (Internet)

#### 📦 Sample DNS Response Packet

- **Transaction ID**: 0x1a2b
- **Flags**: Standard query response, No error (0x8180)
- **Questions**: 1
- **Answer RRs**: 1
- **Answer Name**: `www.google.com`
- **Type**: A
- **Class**: IN
- **Time to Live (TTL)**: e.g., 300 seconds
- **Data Length**: 4
- **Address**: `142.250.183.132`

---

### 3. 🛰️ Internet Control Message Protocol (ICMP)

**Purpose**: Used for diagnostic or control purposes, such as `ping`.

#### 📦 Sample ICMP Echo Request

- **Frame Length**: ~98 bytes
- **Type**: 8 (Echo request)
- **Code**: 0
- **Checksum**: Valid
- **Identifier (BE)**: 0x0001
- **Sequence Number (BE)**: 0x0001
- **Data**: 56 bytes of payload

#### 📦 Sample ICMP Echo Reply

- **Type**: 0 (Echo reply)
- **Code**: 0
- **Checksum**: Valid
- **Identifier**: Same as request
- **Sequence Number**: Same as request
- **Data**: 56 bytes (echoed back)

#### 📝 Observations

- Round-trip times measured
- Confirmed reachability of Google IP (`142.250.xxx.xxx`)

---

## 📁 File Summary

| File | Description |
|------|-------------|
| `Wireshark eth0 Interface.jpg` | Interface selection showing traffic capture via `eth0`. |
| `60s Google Traffic Capturing.jpg` | 60-second general snapshot of captured traffic. |
| `60s Google Traffic Capturing.pcapng` | All captured packets (pcap file). Can only be opened with Wireshark |
| `Google's ARP Traffic Capturing.jpg` | ARP request and response packets. |
| `Google's DNS Traffic Capturing.jpg` | DNS name resolution process for Google. |
| `Google's ICMP Traffic Capturing 1–6.jpg` | Multiple ICMP echo requests and replies for testing connectivity. |

---

## 🧠 Summary

This analysis emphasizes the foundational, non-application-layer protocols in network communication:

- **ARP**: Enables MAC address resolution within a LAN.
- **DNS**: Converts domain names to IPs for server reachability.
- **ICMP**: Diagnoses connection availability and latency.

Together, these allow the OS and network stack to initiate basic communication with any remote server — in this case, Google.

---

## 🛠️ Tools Used

- **Wireshark**: For capturing and analyzing packet-level data.
- **Linux Terminal**: For sending `ping` and DNS queries.
- **Interface**: Ethernet (`eth0`)

---
