# 🌐 Network Plan

## IP Range: 192.168.1.0/24

| Range | Purpose |
|-------|---------|
| .1 | Default Gateway (Router) |
| .10 – .19 | Domain Controllers |
| .20 – .29 | DHCP Servers |
| .30 – .39 | File Servers |
| .40 – .49 | Proxmox / Virtualization |
| .50 – .127 | Reserved (servers, equipment) |
| .128 – .254 | Clients (DHCP) |

---

## Device Table

| Hostname | IP Address | Role | Port |
|----------|-----------|------|------|
| alpha.local | 192.168.1.71 | Proxmox Node 1 | :8006 |
| beta.local | 192.168.1.70 | Proxmox Node 2 | :8006 |
| raspberrypi | 192.168.1.3 | QDevice (Quorum) | – |
| TL-SG105E | 192.168.1.28 | Managed Switch | – |
| main_computer | 192.168.1.27 | Workstation | – |
| DC01 | 192.168.1.10 | Domain Controller | – |
| DC02 | 192.168.1.11 | Domain Controller (backup) | – |
| DHCP01 | 192.168.1.20 | DHCP Server | – |
| DHCP02 | 192.168.1.21 | DHCP Server (backup) | – |
| FS01 | 192.168.1.30 | File Server | – |

---

## VLAN Schema (TP-Link TL-SG105E)

| VLAN ID | Name | Purpose |
|---------|------|---------|
| 10 | LAN | Main network – router, PC, Proxmox management |
| 99 | Heartbeat | Corosync traffic between Proxmox nodes |

### Port Configuration

| Port | Device | PVID | Untagged | Tagged |
|------|--------|------|----------|--------|
| 1 | Router | 10 | VLAN 10 | – |
| 2 | Workstation | 10 | VLAN 10 | – |
| 3 | alpha.local | 10 | – | VLAN 10, 99 |
| 4 | beta.local | 10 | – | VLAN 10, 99 |
| 5 | Reserved | 1 | – | – |

---

## Network Diagram

```
Internet
    │
Router (192.168.1.1) ── Port 1
    │
TL-SG105E Switch (192.168.1.28)
    ├── Port 2 ──► Workstation     (192.168.1.27)
    ├── Port 3 ──► alpha.local     (192.168.1.71)
    ├── Port 4 ──► beta.local      (192.168.1.70)
    └── Port 5 ──► RPi QDevice     (192.168.1.3)

my-cluster
├── alpha  ── 1 vote
├── beta   ── 1 vote
└── QDevice── 1 vote  →  Quorum = 2/3
```
