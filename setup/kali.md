# 🐉 Kali Linux Setup Guide

## Overview
Kali Linux 2025.4 serves as the attacker machine in this lab, connected to the internal LAN segment behind pfSense.

---

## VM Settings

| Setting | Value |
|---------|-------|
| Guest OS | Debian 64-bit |
| RAM | 2GB+ recommended |
| Network Adapter | Custom → VMnet1 |

> VMnet1 connects Kali to the pfSense LAN segment (192.168.1.0/24)

---

## Network Verification

After boot, verify network configuration:

```bash
# Check IP address (should be 192.168.1.x)
ip a

# Test gateway (pfSense)
ping 192.168.1.1

# Test internet connectivity through pfSense
ping 8.8.8.8
ping google.com
```

Expected output:
```
eth0: inet 192.168.1.101/24
Gateway: 192.168.1.1 (pfSense LAN)
```

---

## Tools Used in This Lab

| Tool | Purpose | Install |
|------|---------|---------|
| Nmap | Network scanning | Pre-installed |
| Metasploit | Exploitation framework | Pre-installed |
| Hydra | Brute force attacks | Pre-installed |
| Nikto | Web vulnerability scanner | Pre-installed |
| Dirb | Directory enumeration | Pre-installed |
| Wireshark | Packet analysis | Pre-installed |

---

## Connectivity to Targets

```bash
# Verify Metasploitable2 is reachable
ping 192.168.1.103

# Quick port check
nmap -sn 192.168.1.0/24
```

All traffic between Kali and Metasploitable2 passes through pfSense and is inspected by Suricata.
