# 🖥️ IT Home Lab — Enterprise IT Infrastructure

![Windows Server](https://img.shields.io/badge/Windows%20Server-2022-0078D4?style=flat&logo=windows&logoColor=white)
![Windows 11](https://img.shields.io/badge/Windows%2011-Pro-0078D4?style=flat&logo=windows&logoColor=white)
![VirtualBox](https://img.shields.io/badge/VirtualBox-7.x-183A61?style=flat&logo=virtualbox&logoColor=white)
![Active Directory](https://img.shields.io/badge/Active%20Directory-lab.local-0078D4?style=flat&logo=microsoft&logoColor=white)
![Azure](https://img.shields.io/badge/Azure-for%20Students-0089D6?style=flat&logo=microsoftazure&logoColor=white)
![Status](https://img.shields.io/badge/Status-In%20Progress-yellow?style=flat)

A hands-on home lab built to simulate enterprise-level IT infrastructure and develop practical skills for Level 1 & Level 2 IT Support roles. All configurations were performed from scratch in a fully virtualized environment using VirtualBox.

---

## 📋 Overview

| Detail | Value |
|---|---|
| **Domain** | lab.local |
| **Domain Controller** | DC01 — Windows Server 2022 |
| **Client Machine** | Windows 11 Pro |
| **Network** | NAT (192.168.1.0/24) |
| **DC01 IP** | 192.168.1.10 (Static) |
| **Client IP** | 192.168.1.50 (DHCP) |
| **Cloud Platform** | Microsoft Azure for Students |

---

## 🏗️ Lab ArchitectureInternet (8.8.8.8)
↓
VirtualBox NAT Router (192.168.1.1)
↓
┌─────────────────────────────────────┐
│         NAT Network 192.168.1.0/24  │
│                                     │
│  DC01 (192.168.1.10)                │
│  ├── Active Directory (lab.local)   │
│  ├── DNS Server + Forwarder         │
│  ├── DHCP Server                    │
│  └── VPN Server (L2TP/IPsec)       │
│                                     │
│  Windows 11 Pro (192.168.1.50)      │
│  ├── Domain joined (lab.local)      │
│  └── VPN Client (L2TP connected)   │
└─────────────────────────────────────┘

---

## ✅ Phases Completed

### Phase 1 — Domain Controller & DNS
- Installed Windows Server 2022 and promoted to Domain Controller
- Created domain: `lab.local`
- Configured DNS forwarder to `8.8.8.8` for external resolution
- Joined Windows 11 Pro VM to the domain

### Phase 2 — Active Directory Management
- Created Organisational Units: **IT** and **HR**
- Created Security Groups: **IT ADMIN** and **HR ADMIN**
- Created and managed user accounts (utsarga, larry Wheels)
- Performed: password resets, account lockout/unlock, disable/enable, OU moves, group membership management

### Phase 3 — Group Policy (GPO)
- **IT Wallpaper Policy** — enforces standard desktop wallpaper for IT OU users
- **Block Control Panel** — prevents HR OU users from accessing Control Panel and PC Settings
- Tested with `gpupdate /force` and verified policy application
- Reviewed GPO inheritance and scope using Group Policy Management Console

### Phase 4 — Networking (DHCP, DNS, Commands)
- Installed and configured DHCP Server role on DC01
- Created DHCP scope: `192.168.1.50 — 192.168.1.100`
- Configured scope options: Router (192.168.1.1), DNS (192.168.1.10), Domain (lab.local)
- Disabled VirtualBox built-in DHCP to prevent IP conflicts
- Created manual DNS A records: `printer` and `fileserver`
- Practised: `ipconfig /all`, `ipconfig /release & /renew`, `ping`, `nslookup`, `tracert`, `netstat -an`

### Phase 5 — Helpdesk Ticket Simulation
Resolved 5 real-world IT support scenarios:

| # | Scenario | Resolution |
|---|---|---|
| 01 | Password reset request | Reset in ADUC, enforced change at next logon |
| 02 | Cannot access shared drive | Re-added user to security group |
| 03 | New employee cannot log in | Account was disabled — enabled and configured |
| 04 | No internet (DNS issue) | DHCP renewal corrected DNS server assignment |
| 05 | Account locked after holiday | Unlocked account, identified cached credentials as cause |

### Phase 7 — VPN Server (L2TP/IPsec)
- Installed Routing and Remote Access Service (RRAS) on DC01
- Configured L2TP/IPsec VPN with pre-shared key authentication
- Enabled L2TP WAN Miniport (5 ports for inbound connections)
- Granted VPN Dial-in permission to domain user via ADUC
- Applied Windows 11 NAT registry fix for L2TP compatibility
- Successfully connected Windows 11 VM via L2TP tunnel authenticated through Active Directory

**Troubleshooting experience:** Resolved multiple VPN configuration issues including missing L2TP ports, Dial-in permission set to Deny, and registry fix requirement — demonstrating systematic IT troubleshooting methodology.

### Phase 10 — Security Fundamentals
- Reviewed Security Event Logs in Event Viewer on DC01 (Event IDs: 4624, 4625, 4740, 4720, 4726, 4728)
- Performed Windows Defender Quick Scan and verified definition currency
- Created custom Windows Firewall inbound rule to block ICMP (ping) — verified with `ping` test
- Reviewed UAC levels and their enterprise implications
- Documented common security threats and IT response procedures
