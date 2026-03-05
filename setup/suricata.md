# 🚨 Suricata IDS/IPS Setup Guide

## Overview
Suricata is an open-source Network IDS/IPS installed as a pfSense package. It monitors LAN traffic and generates alerts for suspicious activity.

---

## Installation

**System → Package Manager → Available Packages:**
```
Search: suricata → Install → Confirm
```

---

## Global Settings

**Services → Suricata → Global Settings:**

| Setting | Value |
|---------|-------|
| ETOpen Emerging Threats Rules | ✅ Enabled |
| Snort GPLv2 Community Rules | ✅ Enabled |
| Snort Subscriber Rules | ❌ (requires paid license) |

Save, then update rules:
```
Services → Suricata → Updates → Update Rules
```

---

## Interface Configuration

**Services → Suricata → Interfaces → Add:**

| Setting | Value |
|---------|-------|
| Enable | ✅ |
| Interface | LAN |
| Description | LAN Suricata |
| Send Alerts to System Log | ✅ |
| Block Offenders | ✅ |
| EVE JSON Log | ✅ |

Save → Start (▶ button)

---

## Verified Configuration

```
Interface:      LAN (em1)
Status:         ✅ Running
Pattern Match:  AUTO
Blocking Mode:  LEGACY MODE
Rules:          ETOpen + Snort GPLv2
```

---

## Alert Categories Observed

| Alert Type | Protocol | Description |
|-----------|----------|-------------|
| HTTP Host header ambiguous | TCP/80 | Malformed HTTP requests |
| HTTP Host header invalid | TCP/80 | Invalid HTTP headers |
| SMB malformed request | TCP/445 | Malformed SMB packets |
| TLS invalid record type | TCP/21 | TLS anomaly on FTP port |
| Applayer Mismatch protocol | TCP | Protocol mismatch detection |
| Applayer Detect protocol one direction | TCP | One-way protocol detection |
| ICMPv4 unknown code | ICMP | Unknown ICMP code |

---

## Viewing Alerts

```
Services → Suricata → Alerts
```

Alerts show: Date, Action, Priority, Protocol, Class, Source IP/Port, Destination IP/Port, GID:SID, Description.
