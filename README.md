# 🖥️ IT Home Lab — Enterprise IT Infrastructure

![Windows Server](https://img.shields.io/badge/Windows%20Server-2022-0078D4?style=flat&logo=windows&logoColor=white)
![Windows 11](https://img.shields.io/badge/Windows%2011-Pro-0078D4?style=flat&logo=windows&logoColor=white)
![VirtualBox](https://img.shields.io/badge/VirtualBox-7.x-183A61?style=flat&logo=virtualbox&logoColor=white)
![Active Directory](https://img.shields.io/badge/Active%20Directory-lab.local-0078D4?style=flat&logo=microsoft&logoColor=white)
![Azure](https://img.shields.io/badge/Azure%20Entra%20ID-Student829-0089D6?style=flat&logo=microsoftazure&logoColor=white)
![Altaro](https://img.shields.io/badge/Backup-Altaro%20%7C%20Windows%20Backup-green?style=flat&logo=databricks&logoColor=white)
![Status](https://img.shields.io/badge/Status-In%20Progress-yellow?style=flat)

A hands-on home lab built from scratch to simulate enterprise-level IT infrastructure. Designed to develop and demonstrate practical skills for Level 1 & Level 2 IT Support roles. Every configuration, troubleshooting step, and outcome is documented with screenshots.

---

## 📋 Lab Overview

| Detail | Value |
|---|---|
| **Domain** | lab.local |
| **Domain Controller** | DC01 — Windows Server 2022 |
| **Client Machine** | Windows 11 Pro |
| **Network** | NAT (192.168.1.0/24) |
| **DC01 IP** | 192.168.1.10 (Static) |
| **Client IP** | 192.168.1.50 (DHCP) |
| **Cloud Tenant** | Student829.onmicrosoft.com |
| **Cloud Platform** | Microsoft Azure for Students ($100 credit) |
| **Cloud Admin Role** | Global Administrator |

---

<img width="387" height="617" alt="image" src="https://github.com/user-attachments/assets/69f9b714-8b1c-473c-a4ba-37bef599b6e9" />

## ✅ Phases Completed

### 🔵 Phase 1 — Domain Controller & DNS
- Installed Windows Server 2022 and promoted to Domain Controller
- Created domain: `lab.local` with static IP `192.168.1.10`
- Configured DNS Manager with forwarder to `8.8.8.8` for external resolution
- Joined Windows 11 Pro VM to the domain
- Verified internet + domain name resolution working simultaneously

### 🟢 Phase 2 — Active Directory Management
- Created Organisational Units: **IT** and **HR**
- Created Security Groups: **IT ADMIN** and **HR ADMIN**
- Created and managed users: utsarga Dhakali (IT), Larry Wheels (HR)
- Performed daily AD tasks:
  - Password resets with forced change at next logon
  - Account lockout investigation and unlock
  - Account disable and enable (offboarding/onboarding)
  - Moving users between OUs
  - Adding users to security groups
  - Granting VPN Dial-in permission

### 🟡 Phase 3 — Group Policy (GPO)
- **IT Wallpaper Policy** — enforces standard desktop background for IT OU, prevents user changes
- **Block Control Panel** — prevents HR OU users accessing Control Panel and PC Settings
- Tested with `gpupdate /force` and verified policy application
- Reviewed GPO inheritance and Scope tab in Group Policy Management Console

### 🟠 Phase 4 — Networking (DHCP, DNS, Commands)
- Installed and configured DHCP Server role on DC01
- Created DHCP scope: `192.168.1.50 — 192.168.1.100`
- Configured scope options: Router (192.168.1.1), DNS (192.168.1.10), Domain (lab.local)
- Disabled VirtualBox built-in DHCP to prevent IP conflicts
- Created manual DNS A records: `printer → 192.168.1.200`, `fileserver → 192.168.1.201`
- Practised and documented network commands:

| Command | Purpose |
|---|---|
| `ipconfig /all` | View full network config including DHCP, DNS, MAC |
| `ipconfig /release` | Release current DHCP IP lease |
| `ipconfig /renew` | Request new IP from DHCP server |
| `ipconfig /flushdns` | Clear local DNS cache |
| `ping 8.8.8.8` | Test internet connectivity without DNS |
| `nslookup <name>` | Verify DNS name resolution |
| `tracert <host>` | Trace network route to destination |
| `netstat -an` | View all active connections and ports |

### 🔴 Phase 5 — Helpdesk Ticket Simulation
Resolved 5 real-world IT support scenarios following the ITIL-aligned process:

| # | Scenario | Root Cause | Resolution |
|---|---|---|---|
| 01 | Password reset | Expired/forgotten password | Reset in ADUC with forced change at next logon |
| 02 | Cannot access shared drive | Removed from security group | Re-added user to correct group in ADUC |
| 03 | New employee cannot log in | Account was disabled | Enabled account, verified OU and group |
| 04 | No internet — DNS issue | DHCP gave wrong DNS | `ipconfig /release /renew /flushdns` |
| 05 | Account locked after holiday | Mobile cached old credentials | Unlocked account, advised to update all devices |

### 🟣 Phase 6 — Microsoft Azure Entra ID Administration
- Activated Azure for Students ($100 credit, 365 days, no credit card)
- Accessed Entra ID tenant with **Global Administrator** role
- Managed cloud users: create, review, edit, reset passwords
- Created and managed groups:
  - `Help desk team` (Security group)
  - `IT TEAM` (Microsoft 365 group)
  - `All Company` (Microsoft 365 group)
- Added members to security groups
- Assigned **Helpdesk Administrator** role (RBAC)
- Analysed **Sign-in logs** — reviewed location, IP, auth method, status
- Performed **cloud password reset** on utd1 d1
- Identified MFA gap — single-factor auth detected, MFA recommended
- Documented Free vs P1/P2 licensing differences (RBAC group assignment)

### 🔵 Phase 7 — VPN Setup (L2TP/IPsec)
- Installed Routing and Remote Access Service (RRAS) role on DC01
- Configured L2TP/IPsec VPN server with pre-shared key authentication
- Enabled L2TP WAN Miniport (5 ports for inbound connections)
- Granted VPN Dial-in permission to domain user via ADUC
- Applied Windows 11 NAT registry fix for L2TP compatibility:

reg add "HKLM\SYSTEM\CurrentControlSet\Services\PolicyAgent" /v AssumeUDPEncapsulationContextOnSendRule /t REG_DWORD /d 2 /f



 Successfully connected Windows 11 VM via L2TP tunnel — authenticated through Active Directory

**Troubleshooting experience documented:**

| Issue | Root Cause | Fix |
|---|---|---|
| L2TP security layer error | Registry not applied/VM not restarted | Applied fix, restarted VM |
| IKE credentials unacceptable | IKEv2 needs certificates not PSK | Reverted to L2TP/IPsec |
| No L2TP ports | WAN Miniport had 0 ports | Enabled inbound connections, set 5 ports |
| Dial-in access denied | Permission set to Deny | Changed to Allow in ADUC |

### 🟤 Phase 8 — VM Snapshots & Backup Solutions
**Snapshots (VirtualBox):**
- Took Snapshot 1 of DC01 — clean baseline state captured
- Created test user in Active Directory (testsnap)
- Restored Snapshot 1 — testsnap user confirmed absent, rollback successful ✅
- Documented snapshot best practices and naming conventions

**Windows Server Backup:**
- Installed Windows Server Backup feature on DC01
- Configured backup schedule (11 PM daily)
- Documented backup types: Full, Incremental, Differential, System State
- Explained why backup destination must be a different physical device

**Altaro Backup (Enterprise VM Backup):**
- Documented full Altaro architecture (Agent + Console + Repository + Offsite)
- Step-by-step installation and configuration guide
- Daily monitoring workflow (8-step checklist)
- Restore procedures: Full VM, Granular File, Sandbox, Offsite
- Common troubleshooting scenarios with resolutions
- Altaro vs Windows Server Backup comparison

### 🟥 Phase 10 — Security Fundamentals
- Reviewed Security Event Logs on DC01 in Event Viewer
- Key Event IDs memorised: 4624, 4625, 4740, 4720, 4726, 4728
- Performed Windows Defender Quick Scan — zero threats, definitions current
- Reviewed all three firewall profiles (Domain, Private, Public)
- Created custom inbound firewall rule to block ICMP (ping)
- Verified rule with `ping` — Request timed out confirmed ✅
- Reviewed UAC levels and enterprise implications
- Documented 6 common security threats with IT response procedures

---

## ⏳ Phases Planned

| Phase | Description | Status |
|---|---|---|
| Phase 9 | Ticketing Systems — ServiceNow, Kaseya, SLA theory | 📋 Theory Complete |
| Phase 11 | Microsoft 365 Admin — Exchange Online, SharePoint, Teams, OneDrive | 📋 Planned |
| Phase 12 | Interview Preparation & Scenario Practice | 📋 Planned |

---

#Here's your updated README covering all phases including the new ones!

markdown# 🖥️ IT Home Lab — Enterprise IT Infrastructure

![Windows Server](https://img.shields.io/badge/Windows%20Server-2022-0078D4?style=flat&logo=windows&logoColor=white)
![Windows 11](https://img.shields.io/badge/Windows%2011-Pro-0078D4?style=flat&logo=windows&logoColor=white)
![VirtualBox](https://img.shields.io/badge/VirtualBox-7.x-183A61?style=flat&logo=virtualbox&logoColor=white)
![Active Directory](https://img.shields.io/badge/Active%20Directory-lab.local-0078D4?style=flat&logo=microsoft&logoColor=white)
![Azure](https://img.shields.io/badge/Azure%20Entra%20ID-Student829-0089D6?style=flat&logo=microsoftazure&logoColor=white)
![Altaro](https://img.shields.io/badge/Backup-Altaro%20%7C%20Windows%20Backup-green?style=flat&logo=databricks&logoColor=white)
![Status](https://img.shields.io/badge/Status-In%20Progress-yellow?style=flat)

A hands-on home lab built from scratch to simulate enterprise-level IT infrastructure. Designed to develop and demonstrate practical skills for Level 1 & Level 2 IT Support roles. Every configuration, troubleshooting step, and outcome is documented with screenshots.

---

<img width="492" height="906" alt="image" src="https://github.com/user-attachments/assets/19522463-2d1e-427a-8c35-9fe89bef92e8" />

---

## ✅ Phases Completed

### 🔵 Phase 1 — Domain Controller & DNS
- Installed Windows Server 2022 and promoted to Domain Controller
- Created domain: `lab.local` with static IP `192.168.1.10`
- Configured DNS Manager with forwarder to `8.8.8.8` for external resolution
- Joined Windows 11 Pro VM to the domain
- Verified internet + domain name resolution working simultaneously

### 🟢 Phase 2 — Active Directory Management
- Created Organisational Units: **IT** and **HR**
- Created Security Groups: **IT ADMIN** and **HR ADMIN**
- Created and managed users: utsarga Dhakali (IT), Larry Wheels (HR)
- Performed daily AD tasks:
  - Password resets with forced change at next logon
  - Account lockout investigation and unlock
  - Account disable and enable (offboarding/onboarding)
  - Moving users between OUs
  - Adding users to security groups
  - Granting VPN Dial-in permission

### 🟡 Phase 3 — Group Policy (GPO)
- **IT Wallpaper Policy** — enforces standard desktop background for IT OU, prevents user changes
- **Block Control Panel** — prevents HR OU users accessing Control Panel and PC Settings
- Tested with `gpupdate /force` and verified policy application
- Reviewed GPO inheritance and Scope tab in Group Policy Management Console

### 🟠 Phase 4 — Networking (DHCP, DNS, Commands)
- Installed and configured DHCP Server role on DC01
- Created DHCP scope: `192.168.1.50 — 192.168.1.100`
- Configured scope options: Router (192.168.1.1), DNS (192.168.1.10), Domain (lab.local)
- Disabled VirtualBox built-in DHCP to prevent IP conflicts
- Created manual DNS A records: `printer → 192.168.1.200`, `fileserver → 192.168.1.201`
- Practised and documented network commands:

| Command | Purpose |
|---|---|
| `ipconfig /all` | View full network config including DHCP, DNS, MAC |
| `ipconfig /release` | Release current DHCP IP lease |
| `ipconfig /renew` | Request new IP from DHCP server |
| `ipconfig /flushdns` | Clear local DNS cache |
| `ping 8.8.8.8` | Test internet connectivity without DNS |
| `nslookup <name>` | Verify DNS name resolution |
| `tracert <host>` | Trace network route to destination |
| `netstat -an` | View all active connections and ports |

### 🔴 Phase 5 — Helpdesk Ticket Simulation
Resolved 5 real-world IT support scenarios following the ITIL-aligned process:

| # | Scenario | Root Cause | Resolution |
|---|---|---|---|
| 01 | Password reset | Expired/forgotten password | Reset in ADUC with forced change at next logon |
| 02 | Cannot access shared drive | Removed from security group | Re-added user to correct group in ADUC |
| 03 | New employee cannot log in | Account was disabled | Enabled account, verified OU and group |
| 04 | No internet — DNS issue | DHCP gave wrong DNS | `ipconfig /release /renew /flushdns` |
| 05 | Account locked after holiday | Mobile cached old credentials | Unlocked account, advised to update all devices |

### 🟣 Phase 6 — Microsoft Azure Entra ID Administration
- Activated Azure for Students ($100 credit, 365 days, no credit card)
- Accessed Entra ID tenant with **Global Administrator** role
- Managed cloud users: create, review, edit, reset passwords
- Created and managed groups:
  - `Help desk team` (Security group)
  - `IT TEAM` (Microsoft 365 group)
  - `All Company` (Microsoft 365 group)
- Added members to security groups
- Assigned **Helpdesk Administrator** role (RBAC)
- Analysed **Sign-in logs** — reviewed location, IP, auth method, status
- Performed **cloud password reset** on utd1 d1
- Identified MFA gap — single-factor auth detected, MFA recommended
- Documented Free vs P1/P2 licensing differences (RBAC group assignment)

### 🔵 Phase 7 — VPN Setup (L2TP/IPsec)
- Installed Routing and Remote Access Service (RRAS) role on DC01
- Configured L2TP/IPsec VPN server with pre-shared key authentication
- Enabled L2TP WAN Miniport (5 ports for inbound connections)
- Granted VPN Dial-in permission to domain user via ADUC
- Applied Windows 11 NAT registry fix for L2TP compatibility:
reg add "HKLM\SYSTEM\CurrentControlSet\Services\PolicyAgent" /v AssumeUDPEncapsulationContextOnSendRule /t REG_DWORD /d 2 /f
- Successfully connected Windows 11 VM via L2TP tunnel — authenticated through Active Directory

**Troubleshooting experience documented:**

| Issue | Root Cause | Fix |
|---|---|---|
| L2TP security layer error | Registry not applied/VM not restarted | Applied fix, restarted VM |
| IKE credentials unacceptable | IKEv2 needs certificates not PSK | Reverted to L2TP/IPsec |
| No L2TP ports | WAN Miniport had 0 ports | Enabled inbound connections, set 5 ports |
| Dial-in access denied | Permission set to Deny | Changed to Allow in ADUC |

### 🟤 Phase 8 — VM Snapshots & Backup Solutions
**Snapshots (VirtualBox):**
- Took Snapshot 1 of DC01 — clean baseline state captured
- Created test user in Active Directory (testsnap)
- Restored Snapshot 1 — testsnap user confirmed absent, rollback successful ✅
- Documented snapshot best practices and naming conventions

**Windows Server Backup:**
- Installed Windows Server Backup feature on DC01
- Configured backup schedule (11 PM daily)
- Documented backup types: Full, Incremental, Differential, System State
- Explained why backup destination must be a different physical device

**Altaro Backup (Enterprise VM Backup):**
- Documented full Altaro architecture (Agent + Console + Repository + Offsite)
- Step-by-step installation and configuration guide
- Daily monitoring workflow (8-step checklist)
- Restore procedures: Full VM, Granular File, Sandbox, Offsite
- Common troubleshooting scenarios with resolutions
- Altaro vs Windows Server Backup comparison

### 🟥 Phase 10 — Security Fundamentals
- Reviewed Security Event Logs on DC01 in Event Viewer
- Key Event IDs memorised: 4624, 4625, 4740, 4720, 4726, 4728
- Performed Windows Defender Quick Scan — zero threats, definitions current
- Reviewed all three firewall profiles (Domain, Private, Public)
- Created custom inbound firewall rule to block ICMP (ping)
- Verified rule with `ping` — Request timed out confirmed ✅
- Reviewed UAC levels and enterprise implications
- Documented 6 common security threats with IT response procedures

---

## ⏳ Phases Planned

| Phase | Description | Status |
|---|---|---|
| Phase 9 | Ticketing Systems — ServiceNow, Kaseya, SLA theory | 📋 Theory Complete |
| Phase 11 | Microsoft 365 Admin — Exchange Online, SharePoint, Teams, OneDrive | 📋 Planned |
| Phase 12 | Interview Preparation & Scenario Practice | 📋 Planned |

---

## 📂 Repository Structure
it-home-lab/
│
├── README.md
│
├── documentation/
│   ├── IT_HomeLab_Documentation_v3.docx     ← Full lab guide (all phases)
│   └── Phase6_Phase8_Documentation.docx     ← Azure AD & Backup deep dive
│
├── phase-1-dns/
├── phase-2-active-directory/
├── phase-3-group-policy/
├── phase-4-networking/
├── phase-5-helpdesk/
├── phase-6-azure-entra-id/
├── phase-7-vpn/
├── phase-8-backups/
└── phase-10-security/

---

## 🛠️ Technologies Used

**On-Premises Infrastructure**
- Windows Server 2022 (AD DS, DNS, DHCP, RRAS, Windows Server Backup)
- Windows 11 Pro (Domain client, VPN client)
- VirtualBox 7.x (Hypervisor, NAT networking, Snapshot management)

**Cloud & Identity**
- Microsoft Azure for Students
- Microsoft Entra ID (formerly Azure AD) — Free tier, Global Admin
- Azure Entra ID — Users, Groups, Roles, Sign-in Logs

**Protocols & Services**
- LDAP / Active Directory Domain Services
- DNS (Forward Lookup Zones, A Records, Forwarders to 8.8.8.8)
- DHCP (Scopes, Options, Lease management)
- L2TP/IPsec (VPN tunnel + encryption, pre-shared key)
- TCP/IP, ICMP, HTTPS

**Security**
- Windows Defender (Real-time protection, Quick Scan, Ransomware protection)
- Windows Firewall with Advanced Security (Custom inbound rules)
- IPsec Pre-shared Key Authentication
- Group Policy security enforcement
- Azure Entra ID Sign-in Log monitoring
- Security Event Log analysis (Event Viewer)

**Backup Solutions**
- Windows Server Backup (File, System State, Full Server)
- VirtualBox Snapshots (VM checkpoint management)
- Altaro Backup (Architecture, configuration, monitoring — documented)

---


## 👤 About

Built by **Utsarga Dhakali** as part of a self-directed IT career development program targeting Level 1/2 IT Support roles.

Every service, policy, configuration, and troubleshooting step was performed hands-on from scratch — no pre-configured templates. Real errors were encountered, diagnosed, and resolved, providing authentic troubleshooting experience documented throughout.

---

*Last updated: May 2026*

