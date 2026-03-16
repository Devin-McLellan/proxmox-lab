# 🏠 Proxmox Home Lab

A production-quality two-node Proxmox VE high-availability cluster built as a learning environment covering virtualization, networking, Linux, and cybersecurity.

## 📋 Overview

| Component | Details |
|-----------|---------|
| **Cluster name** | my-cluster |
| **Nodes** | alpha.local, beta.local |
| **Quorum** | Raspberry Pi QDevice |
| **Switch** | TP-Link TL-SG105E |
| **Network** | 192.168.1.0/24 |

## 📁 Repository Structure

```
homelab/
├── README.md               # This file
├── network/
│   └── network-plan.md     # IP plan, VLAN schema, device table
├── guides/
│   └── setup-guide.md      # Step-by-step setup completed so far
└── vms/
    └── vm-plan.md          # Planned virtual machines
```

## ✅ Progress

- [x] Proxmox VE installed on both nodes
- [x] VLAN configuration on TP-Link switch
- [x] Proxmox cluster created (my-cluster)
- [x] beta.local joined the cluster
- [x] Raspberry Pi configured as QDevice
- [ ] Create Domain Controllers (DC01, DC02)
- [ ] Create DHCP servers (DHCP01, DHCP02)
- [ ] Create File Server (FS01)

## 🧠 Key Concepts Learned

- **Virtualization** – Running multiple VMs on physical hardware using KVM/Proxmox
- **VLANs** – Network segmentation using 802.1Q tagging
- **Clustering** – High availability with Corosync/Proxmox cluster
- **Quorum** – Split-brain prevention using a QDevice voter

## 🔗 Resources

- [Proxmox VE Documentation](https://pve.proxmox.com/pve-docs/)
- [Proxmox Wiki](https://pve.proxmox.com/wiki/Main_Page)
