# 🖥️ Virtual Machine Plan

Planned VMs to be deployed on the Proxmox cluster.

---

## VM Overview

| VM ID | Hostname | IP | OS | Role | Node | Status |
|-------|----------|----|----|------|------|--------|
| 100 | DC01 | 10.20.0.10 | Windows Server 2025 | Domain Controller (Primary) | alpha | Built, AD DS pending |
| 103 | Client01 | 10.20.0.x (DHCP) | Windows 11 25H2 | Management workstation / personal projects | alpha | Created, OS install pending |
| 200 | OPNsense | LAN: 10.20.0.1 | OPNsense 26.1.2 | Router/firewall | alpha | Running, config pending |
| — | DC02 | 10.20.0.11 | Windows Server 2025 | Domain Controller (Backup) | beta | Planned |
| — | FS01 | 10.20.0.30 | Windows Server | File Server | alpha | Planned |
| — | Ubuntu01 | 10.20.0.x | Ubuntu 24.04 | Docker host | beta | Planned |
| — | Wazuh | 10.20.0.x | Ubuntu | SIEM + EDR | beta | Planned |
| — | Ollama | 10.20.0.x | Ubuntu | Local LLM for AI security labs | beta | Planned |

---

## Deployment Order

```
1. OPNsense  – Router/firewall (done, final config pending)
2. DC01      – Primary domain controller, DNS
3. Client01  – Management workstation
4. DC02      – Backup domain controller
5. FS01      – File server (joined to domain)
6. Ubuntu01  – Docker host
7. Wazuh     – SIEM + EDR
8. Ollama    – AI security labs
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
