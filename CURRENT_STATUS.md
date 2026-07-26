# CURRENT_STATUS.md

> Genererad: 2026-05-22

---

## 1. Filstruktur

```
proxmox-lab/
├── CHANGELOG.md
├── CLAUDE.md
├── CURRENT_STATUS.md          <- denna fil
├── README.md
├── ansible/
│   ├── ansible.cfg
│   ├── inventory/
│   │   ├── group_vars/
│   │   │   ├── all.yml
│   │   │   ├── proxmox.yml
│   │   │   └── windows.yml
│   │   └── hosts.yml
│   └── playbooks/
│       ├── dc.yml
│       ├── proxmox.yml
│       └── site.yml
├── configs/
│   └── DC01/
│       ├── README.md
│       └── config.ps
├── guides/
│   ├── 01-proxmox-setup.md
│   ├── 02-vlan-switch.md
│   ├── 03-cluster-setup.md
│   ├── 04-qdevice-setup.md
│   ├── 05-heartbeat-vlan99.md
│   ├── 06-ansible-setup.md
│   ├── 07-dc01-setup.md
│   ├── opnsense-setup.md
│   └── sessions/
│       ├── README.md
│       ├── 2026-04-11-network-opnsense.md
│       └── 2026-05-12-infrastructure-review.md
├── journal/
│   └── 2026-04-09.md
├── network/
│   ├── network-plan.md
│   └── vlan-design.md
├── security/
│   ├── firewall-rules.md
│   └── hardening.md
└── vms/
    ├── Client01.md
    ├── DC01.md
    ├── opnsense.md
    └── vm-plan.md
```

-
