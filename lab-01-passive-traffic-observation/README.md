# Lab 01: Passive Network Traffic Observation

## Overview

This lab documents a **controlled experiment in passive network traffic observation** using Wireshark.  
The objective is to study how common network protocols behave on the wire during normal client activity, without performing any active interference, man-in-the-middle attacks, or payload manipulation.

The lab focuses on **observing and explaining protocol behavior**, rather than exploiting vulnerabilities.  
It serves as a foundational exercise for developing packet-level intuition required for later security research, detection, and offensive analysis.

---

## Objectives

The specific goals of this lab are to:

- Observe how **DNS queries and responses** appear in plaintext
- Understand **TCP connection establishment** using the three-way handshake
- Examine **TLS handshakes**, including metadata visibility (e.g., SNI)
- Confirm that **HTTPS payloads are encrypted** during transit
- Capture and interpret **ICMP echo request/reply** traffic
- Practice disciplined packet selection and documentation

---

## Environment

This lab was conducted in an isolated virtual environment.

- **Observer VM:** Kali Linux  
  - Wireshark used for packet capture and analysis
- **Client VM:** Ubuntu Linux  
  - Used to generate browser and ICMP traffic
- **Network Mode:** Virtualized NAT / internal network
- **Capture Type:** Passive only

No active manipulation, packet injection, or traffic alteration was performed.

Detailed environment information is available in `environment.md`.

---

## Scope of Traffic Observed

The following protocol categories were intentionally generated and analyzed:

- **DNS**
  - Domain name queries and responses
  - Plaintext visibility prior to encrypted sessions

- **TCP**
  - SYN, SYN-ACK, and ACK packets
  - Connection establishment to port 443

- **TLS**
  - Client Hello and Server Hello messages
  - Server Name Indication (SNI) visibility
  - Encrypted application data

- **ICMP**
  - Echo request and echo reply packets
  - Reachability and latency observation

Payload decryption was **not attempted**.  
The focus remains on **protocol flow and metadata**, not content inspection.

---

## Methodology

- Traffic was generated from the Ubuntu VM using a web browser and basic network utilities.
- Multiple Wireshark capture sessions were recorded during some browser sessions in close successions.
- Capture files were merged using timestamps to preserve chronological order.
- Display filters were applied to isolate relevant protocol behavior.
- Screenshots were curated to retain full protocol context, including packet bytes.

The goal was to **observe first, then interpret**, avoiding assumptions based solely on tools.

---

## Artifacts

This lab includes the following artifacts:

- **PCAP file**
  - `lab01_traffic.pcap` — merged capture representing the full experiment
- **Screenshots**
  - Curated packet views illustrating DNS, TCP, TLS, and ICMP behavior
- **Analysis**
  - Detailed observations and security implications documented in `analysis.md`

---

## Limitations

- Traffic was generated in a lab environment and may differ from real-world enterprise networks.
- DNS was not encrypted (no DoH / DoT).
- QUIC traffic was observed but not analyzed in depth.
- No adversarial behavior was introduced.

These limitations are intentional to maintain clarity and control in a foundational lab.

---

## Why This Lab Matters

Most security failures begin with **incorrect assumptions about how systems communicate**.

This lab builds the habit of:
- reading packets carefully
- understanding protocol intent
- recognizing what information is exposed or protected

before attempting exploitation, automation, or detection logic.

It forms the baseline for future labs involving:
- packet manipulation
- authentication flow analysis
- detection engineering
- malware traffic research
