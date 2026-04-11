# VLAN Design — Proxmox Home Lab

## Översikt

| VLAN | Subnät          | Syfte                         | Internet |
|------|-----------------|-------------------------------|----------|
| 10   | 192.168.1.0/24  | Management (Proxmox, PC, RPi) | Ja       |
| 20   | 10.20.0.0/24    | Lab VMs (isolerat)            | Nej      |
| 99   | 10.99.0.0/24    | Corosync heartbeat            | Nej      |

## TP-Link TL-SG105E — Portkonfiguration

| Port | Enhet        | VLAN-konfiguration                         |
|------|--------------|--------------------------------------------|
| 1    | Router       | VLAN 10 untaggat                           |
| 2    | Workstation  | VLAN 10 untaggat                           |
| 3    | alpha.local  | Trunk: VLAN 10 untaggat, VLAN 20+99 taggat |
| 4    | beta.local   | Trunk: VLAN 10 untaggat, VLAN 20+99 taggat |
| 5    | Raspberry Pi | VLAN 10 untaggat (PVID 10)                 |

## Proxmox Nätverksgränssnitt (per nod)

```
nic0                        → Fysiskt NIC (ingen IP)
nic0.20                     → VLAN 20 sub-interface
nic0.99                     → VLAN 99 sub-interface
vmbr0  → 192.168.1.71/24   → Management bridge (VLAN 10)
vmbr20 → ingen IP           → Lab VM bridge (VLAN 20)
vmbr99 → 192.168.99.71/24  → Corosync bridge (VLAN 99)
```

Beta: samma konfiguration med .70-adresser.

## OPNsense

| Interface | Enhet  | IP               | Syfte                      |
|-----------|--------|------------------|----------------------------|
| WAN       | vtnet0 | 192.168.1.32/24  | Internet (DHCP från router) |
| LAN       | vtnet1 | 10.20.0.1/24     | Gateway för lab-VMs        |

DHCP-pool LAN: 10.20.0.100 – 10.20.0.200
Statiska reservationer (planerade): 10.20.0.2–99
