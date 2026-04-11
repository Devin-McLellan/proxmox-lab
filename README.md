# Proxmox Home Lab

Enterprise-grade two-node Proxmox VE homelab built to develop skills in Detection Engineering, Security Engineering, and AI-powered cybersecurity.

**Goal:** Cybersecurity Specialist at Combitech / Orange Cyberdefense.
**LIA (internship) target:** Autumn 2026.

## Infrastructure

| Component | Details |
|-----------|---------|
| **Cluster** | my-cluster (Proxmox VE 9.1.6) |
| **Nodes** | alpha.local (192.168.1.71), beta.local (192.168.1.70) |
| **Quorum** | Raspberry Pi QDevice (192.168.1.3) |
| **Switch** | TP-Link TL-SG105E (192.168.1.28) |

## Network Design

| VLAN | Subnet | Purpose |
|------|--------|---------|
| VLAN 10 | 192.168.1.0/24 | Management — Proxmox GUI, workstation |
| VLAN 20 | 10.20.0.0/24 | Lab VMs — all servers and clients |
| VLAN 99 | 10.99.0.0/24 | Corosync heartbeat only |

Routing between VLANs is handled by OPNsense VM (VM 200).

## Repository Structure

```
proxmox-lab/
├── guides/          # Step-by-step setup guides (markdown)
├── vms/             # VM configurations and specs
├── network/         # Network diagrams and configs
├── ansible/
│   ├── inventory/   # Hosts and group_vars
│   └── playbooks/   # Automation playbooks
├── security/        # Attack scenarios, ESC1, OWASP LLM Top 10 labs
├── configs/         # Raw config files
└── journal/         # Lab session logs and learning notes
```

## Progress

### Infrastructure

- [x] Proxmox VE 9.1.6 on both nodes
- [x] VLAN 10/20/99 on TP-Link switch (tagged/untagged)
- [x] Proxmox cluster (my-cluster) + beta.local joined
- [x] Raspberry Pi configured as QDevice
- [x] Corosync heartbeat on VLAN 99 (vmbr99)
- [x] WireGuard VPN + DuckDNS (kevins-proxmox.duckdns.org)
- [ ] OPNsense VM — router/firewall for VLAN 20 ← **current**
- [ ] Domain Controllers (DC01, DC02)
- [ ] DHCP / File Server
- [ ] Ansible automation

### Cybersecurity

- [ ] ADCS + ESC1 vulnerable certificate template (Certify.exe)
- [ ] Wazuh SIEM/EDR
- [ ] OWASP LLM Top 10 labs (Ollama + Suricata + LangChain)

## Curriculum

1. ✅ Access Proxmox Management Interface
2. ✅ Lab Architecture & Network Design
3. ✅ Download & Upload ISO images
4. ⏳ OPNsense installation and configuration ← **current**
5. ⏳ Windows Server 2025 & Active Directory (DC01)
6. ⏳ Service Accounts, SPNs, Network Shares
7. ⏳ Windows 10 & Domain Join
8. ⏳ Third Party Apps & Windows Server 2022
9. ⏳ ADCS + ESC1 vulnerable certificate template
10. ⏳ Ubuntu 24.04 & Docker
11. ⏳ Web App via Docker Compose
12. ⏳ Elasticsearch, Kibana & Log Shipping

## AI Security

OWASP LLM Top 10 (2025) homelab labs using Ollama, Wazuh, Suricata, ChromaDB, and OPNsense.
See `security/` for individual lab writeups.

**AI Stack:** Ollama + ChromaDB + LangChain + Wazuh, isolated in VLAN 20 behind OPNsense + Suricata IDS.

## Key Learnings

- VirtIO disk causes Windows install failure — always use SATA for Windows VMs
- VirtIO NIC may not work on Windows — use E1000
- `vmbr0` + VLAN tag 20 conflicts with `nic0.20` — use `vmbr20` directly for lab VMs
- OPNsense must be deployed before domain controllers (provides DHCP + routing for VLAN 20)
- QDevice requires wired connection for stable quorum

## Resources

- [Proxmox VE Documentation](https://pve.proxmox.com/pve-docs/)
- [OPNsense Documentation](https://docs.opnsense.org/)
- [Ansible Documentation](https://docs.ansible.com/)
- [OWASP LLM Top 10](https://owasp.org/www-project-top-10-for-large-language-model-applications/)
