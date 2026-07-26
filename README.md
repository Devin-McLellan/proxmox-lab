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
| VLAN 99 | 192.168.99.0/24 | Corosync heartbeat only |

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

## Resources

- [Proxmox VE Documentation](https://pve.proxmox.com/pve-docs/)
- [OPNsense Documentation](https://docs.opnsense.org/)
- [Ansible Documentation](https://docs.ansible.com/)
- [OWASP LLM Top 10](https://owasp.org/www-project-top-10-for-large-language-model-applications/)
