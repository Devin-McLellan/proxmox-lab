# Proxmox Home Lab — Project Context for Claude Code

> Owner: Kevin (Devin-McLellan)
> Goal: Cybersecurity Specialist targeting Combitech / Orange Cyberdefense within 2-3 years
> Focus: Detection Engineering + Security Engineering with AI specialization
> LIA (Internship) target: Autumn 2026
> GitHub: https://github.com/Devin-McLellan/proxmox-lab

---

## Who I Am

I am a student building a hands-on home lab from scratch to learn IT infrastructure,
virtualization, networking, Linux, and cybersecurity. I prefer step-by-step guidance
with explanations of *why* each step matters. I use PowerShell over GUI where possible
and document everything in markdown afterward.

**Communication:** I write in Swedish but all documentation is in English.
**Tools:** PowerShell, VS Code, Git Bash, Windows environment.
**Secrets:** All credentials stored in Bitwarden (folders: Proxmox Lab, Cloud Services, Windows).

---

## Physical Infrastructure

### Hardware

| Device | IP | Role |
|---|---|---|
| Router | 192.168.1.1 | Internet gateway |
| TP-Link TL-SG105E | 192.168.1.28 | Managed switch, 802.1Q VLAN |
| alpha.local | 192.168.1.71 | Proxmox VE node 1 |
| beta.local | 192.168.1.70 | Proxmox VE node 2 |
| Raspberry Pi | 192.168.1.3 | Proxmox QDevice (quorum) — wired, Port 5 |
| Workstation | 192.168.1.x | Daily-use PC, management workstation |

### Switch Port Layout (TP-Link TL-SG105E)

| Port | Device | VLAN config |
|---|---|---|
| Port 1 | Router | VLAN 10 untagged |
| Port 2 | Workstation | VLAN 10 untagged |
| Port 3 | alpha.local | Trunk: VLAN 10 untagged, VLAN 20+99 tagged |
| Port 4 | beta.local | Trunk: VLAN 10 untagged, VLAN 20+99 tagged |
| Port 5 | Raspberry Pi | VLAN 10 untagged (PVID 10) |

---

## Proxmox Cluster

- **Cluster name:** my-cluster
- **Nodes:** alpha (192.168.1.71) + beta (192.168.1.70)
- **Quorum:** QDevice on Raspberry Pi, 3 votes, Quorate
- **Proxmox version:** 9.1.6
- **Post-install:** Community script run on both nodes (fixed enterprise repo errors)

### Network Configuration (per node)

```
nic0                          Physical NIC (no IP, manual)
nic0.20                       VLAN 20 sub-interface (no IP)
nic0.99                       VLAN 99 sub-interface (no IP)
vmbr0  -> 192.168.1.71/24    Management bridge (VLAN 10, Proxmox GUI)
vmbr20 -> no IP              Lab VM bridge (VLAN 20)
vmbr99 -> 192.168.99.71/24   Corosync heartbeat bridge (VLAN 99)
```

Beta mirrors alpha with .70 addresses.

---

## Network Design

| VLAN | Subnet | Purpose | Internet |
|---|---|---|---|
| VLAN 10 | 192.168.1.0/24 | Management — Proxmox GUI, workstation, QDevice | Yes |
| VLAN 20 | 10.20.0.0/24 | Lab VMs — all servers and clients | No (by default) |
| VLAN 99 | 192.168.99.0/24 | Corosync heartbeat only | No |

Routing between VLANs is handled by OPNsense VM.

---

## Virtual Machines

### Current VMs

| VM ID | Name | Node | OS | Specs | Status |
|---|---|---|---|---|---|
| 100 | DC01 | alpha | Windows Server 2025 | 4c/4GB/60GB SATA | Built, AD DS pending |
| 101 | linux-lab | beta | Linux | — | Exists |
| 102 | Linux-VM-main | beta | Linux | — | Exists |
| 103 | Client01 | alpha | Windows 11 25H2 | 2c/8GB/64GB SATA, UEFI, TPM 2.0 | Created, OS install pending |
| 200 | OPNsense | alpha | FreeBSD (OPNsense 26.1.2) | 2c/2GB/16GB | WAN: 192.168.1.32, LAN: 10.20.0.1 |

### Important VM Notes

- Windows VMs: Always use SATA disk (VirtIO caused install failure)
- Windows VMs: Use E1000 NIC (VirtIO NIC may not work)
- DC01: Currently on vmbr20 with VLAN tag 20
- OPNsense: net0=vmbr0 (WAN), net1=vmbr20 (LAN)

### Planned VMs

| VM | OS | Role |
|---|---|---|
| DC02 | Windows Server 2025 | Secondary domain controller |
| FS01 | Windows Server | File server |
| Client01 | Windows 10 | Domain-joined workstation |
| Ubuntu01 | Ubuntu 24.04 | Docker host |
| Wazuh | Ubuntu | SIEM + EDR |
| Ollama | Ubuntu | Local LLM for AI security labs |

---

## Course Curriculum

1. Done: Access Proxmox Management Interface
2. Done: Lab Architecture and Network Design
3. Done: Download and Upload ISO images
4. Done: OPNsense installation and configuration
5. Current: Install Windows Server 2025 and Active Directory (DC01)
6. Pending: Create Service Accounts, SPNs, Network Shares
7. Pending: Install Windows 10 and Join Domain
8. Pending: Install Third Party Applications and Windows Server 2022
9. Pending: Configure ADCS and Administrator
10. Pending: Create Vulnerable Certificate Template (ESC1) using Certify.exe
11. Pending: Install Ubuntu 24.04 and Docker
12. Pending: Web Application via Docker Compose
13. Pending: Elasticsearch, Kibana and Log Shipping

---

## OPNsense Configuration (Completed)

- WAN: DHCP from router -> 192.168.1.32
- LAN: 10.20.0.1/24 (gateway for all lab VMs)
- DHCP pool: 10.20.0.100-200
- Hostname: OPNsense, Domain: lab.local, Timezone: Europe/Stockholm

Pending:
- Reserve static WAN-IP (192.168.1.30) in router
- Firewall rule: permanent VLAN 10 -> VLAN 20 management access
- Suricata IDS/IPS: AI security phase
- Rate limiting: Ollama protection (LLM10)

---

## AI + Cybersecurity Learning Plan

### Certifications Priority

| Cert | Priority |
|---|---|
| CompTIA Security+ | High — first to complete |
| GIAC GCIH | High — Incident Handling |
| SC-200 (Microsoft) | Medium — Sentinel/AI-SIEM |
| GIAC GCIA | Medium — Network analysis |

### OWASP LLM Top 10 (2025) Homelab Tasks

| ID | Vulnerability | Homelab Task | Tools |
|---|---|---|---|
| LLM01 | Prompt Injection | Test injection against Ollama | Garak, Burp Suite, Python |
| LLM02 | Sensitive Info Disclosure | Build chatbot, extract context | Presidio, LangFuse, Wazuh |
| LLM03 | Supply Chain | Isolate AI stack, verify with Suricata | Suricata, Proxmox VLAN, Trivy |
| LLM04 | Data Poisoning | Fine-tune model on manipulated data | Hugging Face, CleanLab |
| LLM05 | Improper Output Handling | Flask app, trigger XSS via prompts | Flask, OWASP ZAP, Semgrep |
| LLM06 | Excessive Agency | LLM agent with limited tool access | LangChain, Ollama, Wazuh |
| LLM07 | System Prompt Leakage | Build chatbot, extract system prompt | Garak, Python |
| LLM08 | Vector/Embedding Weaknesses | RAG pipeline, manipulate results | ChromaDB, Ollama, LangChain |
| LLM09 | Misinformation | Test Ollama on CVE questions vs NVD | Ollama, NVD API, Python |
| LLM10 | Unbounded Consumption | Rate limiting OPNsense vs Ollama | OPNsense, Wazuh, cAdvisor |

---

## GitHub Repository Structure

```
proxmox-lab/
├── guides/
│   ├── sessions/    -> Per-session logs (one file per evening)
│   └── *.md         -> Step-by-step setup guides
├── vms/             -> VM configurations and specs
├── network/         -> Network diagrams and configs
├── ansible/
│   ├── inventory/   -> Hosts and group_vars
│   └── playbooks/   -> Automation playbooks
├── security/        -> Attack scenarios, ESC1, OWASP labs
├── configs/         -> Raw config files
├── journal/         -> General learning notes
└── CLAUDE.md        -> This file
```

---

## Key Technical Learnings

- VirtIO disk causes Windows install failure: always use SATA for Windows VMs
- VirtIO NIC may not work for Windows: use E1000
- Git does not track empty directories: use .gitkeep
- QDevice stability requires wired connection (not WiFi)
- Proxmox storage: local (ISO/backups) vs local-lvm (VM disks)
- VLAN sub-interfaces (nic0.20) filter tagged traffic; bridges (vmbr20) are the switch VMs connect to
- OPNsense must be deployed BEFORE domain controllers
- pfctl -d disables OPNsense firewall temporarily for troubleshooting (re-enable with pfctl -e)
- Block RFC1918 must be unchecked in OPNsense wizard when LAN is a private subnet

---

## Workflow

Claude (web/project) -> Architecture decisions, learning, guidance
Claude Code (terminal) -> Scripting, Ansible, Python, automation

---

Last updated: May 2026
Current step: OPNsense final configuration — static WAN IP + VLAN 10→20 firewall rule
Next step: Windows Server 2025 and Active Directory (DC01)
