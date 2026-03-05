# 🔥 pfSense Setup Guide

## Overview
pfSense 2.8.1 (FreeBSD-based) acts as the perimeter firewall, router, and IDS host for this lab.

---

## Prerequisites
- VMware Workstation Pro
- pfSense ISO: https://www.pfsense.org/download/
- Minimum: 1 CPU, 1GB RAM, 8GB disk

---

## Step 1 — VMware Virtual Networks

Open **Edit → Virtual Network Editor:**

| Network | Type | DHCP | Purpose |
|---------|------|------|---------|
| VMnet1 | Host-only | ❌ OFF | Internal LAN |
| VMnet8 | NAT | ✅ ON | WAN / Internet |

> ⚠️ Disable DHCP on VMnet1 — pfSense will handle it

---

## Step 2 — pfSense VM Settings

Create VM with **Guest OS: FreeBSD 64-bit**

| Adapter | Network | Role |
|---------|---------|------|
| Network Adapter 1 | Custom → VMnet1 | WAN |
| Network Adapter 2 | Custom → VMnet8 | LAN |

> Note: Due to VMware adapter ordering, em0 maps to VMnet1 (WAN) and em1 maps to VMnet8 (LAN) in this configuration.

---

## Step 3 — Console Setup

On first boot, assign interfaces:

```
1) Assign Interfaces
   WAN → em0
   LAN → em1

2) Set interface(s) IP address
   LAN:
     IP:    192.168.1.1
     Mask:  24
     DHCP:  yes (range 192.168.1.100 - 192.168.1.200)
```

---

## Step 4 — Web Configurator

Access from Kali browser:
```
URL:      https://192.168.1.1
Login:    admin
Password: pfsense (change immediately!)
```

---

## Step 5 — Setup Wizard

| Field | Value |
|-------|-------|
| Hostname | pfsense |
| Domain | home.lab |
| DNS 1 | 8.8.8.8 |
| DNS 2 | 8.8.4.4 |
| Timezone | Your region |
| WAN Type | DHCP |
| LAN IP | 192.168.1.1 |

---

## Step 6 — Advanced Networking (required for Suricata)

**System → Advanced → Networking:**

```
✅ Disable Hardware Checksum Offloading
✅ Disable Hardware TCP Segmentation Offloading
✅ Disable Hardware Large Receive Offloading
```

Save and reboot.

---

## Firewall Rules

Default LAN rules allow all outbound traffic. Custom rules can be added at **Firewall → Rules → LAN**.

| Rule | Protocol | Source | Destination | Action |
|------|----------|--------|-------------|--------|
| Anti-Lockout | TCP | * | LAN Address:443,80 | Allow |
| Default LAN | IPv4 | LAN subnets | * | Allow |
| Default LAN IPv6 | IPv6 | LAN subnets | * | Allow |
