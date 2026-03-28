# Network Plan

## Physical Infrastructure

| Device | Port | IP | VLAN |
|--------|------|----|------|
| Router/Gateway | Port 1 | 192.168.1.1 | 10 (untagged) |
| Workstation | Port 2 | 192.168.1.x | 10 (untagged) |
| alpha.local | Port 3 | 192.168.1.71 | 10 (untagged), 20 (tagged), 99 (tagged) |
| beta.local | Port 4 | 192.168.1.70 | 10 (untagged), 20 (tagged), 99 (tagged) |
| Raspberry Pi (QDevice) | Port 5 | 192.168.1.3 | 10 (untagged) |

## VLAN Configuration

| VLAN | Name | Subnet | Purpose |
|------|------|--------|---------|
| 10 | Management | 192.168.1.0/24 | Proxmox web UI, workstation, RPi |
| 20 | Lab | 192.168.10.0/24 | All VMs (DC01, DC02, DHCP, FS, Ubuntu) |
| 99 | Heartbeat | 192.168.99.0/24 | Corosync cluster communication |

## Switch Port Configuration (TP-Link TL-SG105E)

| Port | PVID | VLAN 10 | VLAN 20 | VLAN 99 |
|------|------|---------|---------|---------|
| 1 (Router) | 10 | Untagged | Not Member | Not Member |
| 2 (Workstation) | 10 | Untagged | Not Member | Not Member |
| 3 (alpha) | 10 | Untagged | Tagged | Tagged |
| 4 (beta) | 10 | Untagged | Tagged | Tagged |
| 5 (RPi) | 10 | Untagged | Not Member | Not Member |

## Proxmox Network Interfaces

| Interface | Type | Bridge Port | IP | Purpose |
|-----------|------|-------------|-----|---------|
| vmbr0 | Linux Bridge | nic0 | 192.168.1.71 / .70 | Management (VLAN 10) |
| nic0.20 | Linux VLAN | nic0 | - | VLAN 20 subinterface |
| vmbr20 | Linux Bridge | nic0.20 | - | Lab VM network |
| nic0.99 | Linux VLAN | nic0 | - | VLAN 99 subinterface |
| vmbr99 | Linux Bridge | nic0.99 | 192.168.99.71 / .70 | Heartbeat (Corosync) |

## IP Address Allocation

| Range | Purpose |
|-------|---------|
| 192.168.1.1 | Gateway/Router |
| 192.168.1.3 | Raspberry Pi (QDevice) |
| 192.168.1.70 | beta.local (Proxmox) |
| 192.168.1.71 | alpha.local (Proxmox) |
| 192.168.99.70 | beta.local (Heartbeat) |
| 192.168.99.71 | alpha.local (Heartbeat) |
| 192.168.10.10-19 | Domain Controllers |
| 192.168.10.20-29 | DHCP Servers |
| 192.168.10.30-39 | File Servers |
| 192.168.10.128-254 | DHCP client range |

## Cluster Quorum

Three-vote system for split-brain prevention:
- alpha: 1 vote
- beta: 1 vote  
- QDevice (RPi): 1 vote
- Quorum requires: 2/3 votes
