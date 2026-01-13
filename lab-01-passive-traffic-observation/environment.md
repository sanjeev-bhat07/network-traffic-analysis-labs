# Lab 01 Environment Details

This document describes the environment used to conduct **Lab 01: Passive Network Traffic Observation**.  
All experiments were performed in a controlled, isolated virtual setup to ensure reproducibility and ethical boundaries.

---

## Host System

- **Host OS:** Windows 11
- **Architecture:** x86_64
- **Virtualization Platform:** VMware Workstation Pro (25H2)

---

## Virtual Machines

### Observer VM

- **Operating System:** Kali Linux
- **Role:** Passive traffic capture and analysis
- **Tools Used:**
  - Wireshark
- **Network Interface:** `eth0`
- **RAM:** 4 GB
- **Storage:** 20 GB

Wireshark was used strictly in passive capture mode.  
No packet injection, manipulation, or forwarding was enabled.

---

### Client VM

- **Operating System:** Ubuntu 22.04 LTS (Jammy Jellyfish)
- **Role:** Traffic generation (client activity)
- **Activities Performed:**
  - Web browsing (HTTP / HTTPS)
  - ICMP echo requests (`ping`)
- **RAM:** 2 GB
- **Storage:** 20 GB

The client VM generated normal user traffic only.  
No attack tools or adversarial activity were used.

---

## Network Configuration

- **Network Mode:** Virtualized NAT / internal network
- **IP Addressing:** Private IP range (assigned dynamically)
- **Traffic Visibility:** Observer VM able to capture traffic within the virtual network segment

All traffic remained within the virtualized environment and did not involve external monitoring of third-party systems.

---

## Capture Configuration

- **Capture Type:** Passive
- **Promiscuous Mode:** Enabled on capture interface
- **Capture Sessions:** Multiple sessions during the same browser activity
- **PCAP Handling:** Individual capture files merged using timestamps to create a single chronological capture

---

## Notes on Reproducibility

This lab can be reproduced by:
- Setting up two virtual machines on the same virtual network using VMware Workstation
- Using one VM to generate normal client traffic
- Using the second VM to passively capture traffic with Wireshark

Minor variations in packet timing, IP addresses, and protocol versions are expected depending on host system and network conditions.
