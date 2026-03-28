# Proxmox Home Lab

A production-quality two-node Proxmox VE high-availability cluster built as a learning environment covering virtualization, networking, Linux, and cybersecurity.

## Overview

| Component | Details |
|-----------|---------|
| **Cluster name** | my-cluster |
| **Nodes** | alpha.local (192.168.1.71), beta.local (192.168.1.70) |
| **Quorum** | Raspberry Pi QDevice (192.168.1.3) |
| **Switch** | TP-Link TL-SG105E (192.168.1.28) |
| **Network** | 192.168.1.0/24 |

## Repository Structure

```
proxmox-lab/
├── README.md
├── network/                    # Network plan, VLAN schema, diagrams
├── guides/                     # Step-by-step setup guides
├── vms/                        # VM documentation and specs
├── ansible/                    # Automation playbooks and roles
├── security/                   # Hardening and firewall rules
└── configs/                    # Backup of configuration files
```

## Progress

- [x] Proxmox VE installed on both nodes
- [x] VLAN configuration on TP-Link switch (VLAN 10, 20, 99 with proper tagged/untagged ports)
- [x] Proxmox cluster created (my-cluster)
- [x] beta.local joined the cluster
- [x] Raspberry Pi configured as QDevice
- [x] Network interfaces configured (nic0.20/vmbr20 for Lab, nic0.99/vmbr99 for Heartbeat)
- [x] Corosync heartbeat migrated to VLAN 99 (192.168.99.x)
- [ ] Create Domain Controllers (DC01, DC02)
- [ ] Create DHCP servers (DHCP01, DHCP02)
- [ ] Create File Server (FS01)
- [ ] Configure Ansible automation
- [ ] Implement security hardening

## Key Concepts

- **Virtualization** – Running multiple VMs on physical hardware using KVM/Proxmox
- **VLANs** – Network segmentation using 802.1Q tagging
- **Clustering** – High availability with Corosync/Proxmox cluster
- **Quorum** – Split-brain prevention using a QDevice voter
- **Ansible** – Infrastructure automation and configuration management

## Resources

- [Proxmox VE Documentation](https://pve.proxmox.com/pve-docs/)
- [Proxmox Wiki](https://pve.proxmox.com/wiki/Main_Page)
- [Ansible Documentation](https://docs.ansible.com/)
