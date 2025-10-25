# Offensive Security & Penetration Testing Lab Environment

**Author:** kaiju0x  
**Maintained:** 2025-10-25  
**Purpose:** A reproducible, isolated virtualization lab for hands-on offensive security practice, pentesting, and red-team exercises. This repository documents architecture, configuration, tooling and deliverables for a controlled lab environment.

---

## Overview
This lab simulates a small enterprise network with multiple attacker and target systems to practice reconnaissance, exploitation, post-exploitation, and reporting workflows. It is intentionally isolated (host-only/internal network) to prevent accidental external scans and to ensure all testing is performed with explicit authorization.

Supported exercises include: Linux and Windows privilege escalation, Active Directory enumeration and exploitation, web application testing, C2 experimentation (in a safe lab), and tooling automation.

---

## Objectives
- Maintain attacker & target VMs for realistic offensive exercises.
- Use an isolated host-only/internal network and create snapshots for safe rollback.
- Install/verify core tooling on attacker VMs (Kali, Parrot) and baseline services on targets (Windows 10, Windows Server, Ubuntu).
- Produce repeatable artifacts: scans (`.nmap`), pcaps, sanitized exploitation writeups, and professional mini-reports for portfolio use.
- Practice ethics: only test systems you own or have explicit permission to test.

---

## Lab VM Inventory

| VM name           | OS / Version                | CPU | RAM  | IP (lab)        | Role / Notes |
|-------------------|-----------------------------|-----|------|------------------|--------------|
| `kali-attacker`   | Kali Linux (latest)         | 2   | 4 GB | 192.168.56.10    | Primary attacker — main tooling |
| `parrot-attacker` | Parrot Security OS (latest) | 2   | 4 GB | 192.168.56.11    | Secondary attacker — tool diversity |
| `win10-target`    | Windows 10 Pro              | 1   | 2 GB | 192.168.56.20    | Workstation target |
| `winserver-target`| Windows Server 2019         | 2   | 4 GB | 192.168.56.21    | Domain Controller / AD lab |
| `ubuntu-target`   | Ubuntu Server 22.04         | 1   | 1 GB | 192.168.56.30    | Linux server target |

> Adjust CPU/RAM based on host capacity. Use static IPs on the host-only network. Replace IPs above with your assigned values.

---

## Per-VM Tooling & Configuration

### Attacker VMs (Kali / Parrot)
Common baseline tooling to install:
- nmap, masscan, netcat, socat
- Wireshark / tcpdump (user added to `wireshark` group)
- Burp Suite Community (manual download & install)
- Python3, pip3, git
- Metasploit-framework (optional)
- BloodHound + Neo4j (for AD graphing) — Neo4j service requires additional setup
- CrackMapExec (CME), Impacket, bloodhound, enum4linux, smbclient, gobuster, amass, subfinder, sliver/mythic/other C2s as needed
- Hashcat, John the Ripper (password cracking)
- Tools you write/maintain in `/tools/` (Python/PowerShell scripts)

Notes:
- Parrot may have slightly different package names/paths — the included `setup_commands.sh` handles the common installs.
- Burp requires manual download from PortSwigger (accept license and install to `~/tools/burpsuite`).

### Target VMs
**Windows Server 2019**
- Configure AD DS + DNS (create domain `lab.internal` or similar)
- Create domain users and groups for enumeration/attack scenarios
- Enable RDP (for convenience) and SMB services
- Create shares and sample files for discovery/exfiltration exercises

**Windows 10**
- Join domain or leave as standalone depending on scenario
- Install Sysinternals (optional), enable RDP if needed

**Ubuntu Server**
- Install `openssh-server`, `apache2`, sample web apps (for web testing), and any services you want to practice on
- Harden baseline per normal server defaults, then intentionally misconfigure things for practice in controlled tests

---

## Setup & Snapshot Naming Conventions
Create snapshots at the following milestones (name exactly as shown for consistency):
- `kali-base`
- `parrot-base`
- `win10-base`
- `winserver-base`
- `ubuntu-base`
- Additional snapshots as you progress: `ad-configured`, `post-enum`, `post-exploit-clean`

Always snapshot **before** running exploit or red-team activities so you can roll back cleanly.

---

## Ethics & Safety
This lab is for authorized, educational use only. Do not run tools, payloads, or scans against systems you do not own or explicitly have permission to test. When publishing, sanitize any outputs that could be linked to live systems or real credentials.

---

## Contact / Repo maintenance
**Repo Owner:** kaiju0x 
**Last updated:** 2025-10-25
