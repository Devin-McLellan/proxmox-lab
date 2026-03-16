# 🖥️ Virtual Machine Plan

Planned VMs to be deployed on the Proxmox cluster.

---

## VM Overview

| VM ID | Hostname | IP | OS | Role | Node |
|-------|----------|----|----|------|------|
| 101 | DC01 | 192.168.1.10 | Windows Server / Samba | Domain Controller (Primary) | alpha |
| 102 | DC02 | 192.168.1.11 | Windows Server / Samba | Domain Controller (Backup) | beta |
| 201 | DHCP01 | 192.168.1.20 | Linux | DHCP Server (Primary) | alpha |
| 202 | DHCP02 | 192.168.1.21 | Linux | DHCP Server (Backup) | beta |
| 301 | FS01 | 192.168.1.30 | Linux | File Server | alpha |

---

## Deployment Order

```
1. DC01  – Primary domain controller, DNS
2. DC02  – Backup domain controller
3. DHCP01 – Primary DHCP
4. DHCP02 – Backup DHCP
5. FS01  – File server (joined to domain)
```

---

## HA Strategy

- **Primary VMs** (DC01, DHCP01) run on **alpha**
- **Backup VMs** (DC02, DHCP02) run on **beta**
- If one node fails, the other node already has a running backup
- QDevice ensures quorum is maintained with only one node online

---

## Notes

- Use **Cloud-Init templates** for fast Linux VM deployment
- Take **snapshots** before major changes
- All VMs should use **VirtIO** drivers for best performance
- Storage: local-lvm (default) until shared storage is configured
