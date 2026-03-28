# Guide 02 â VLAN Switch Configuration (TP-Link TL-SG105E)

## Tool
Configure using **Easy Smart Configuration** (TP-Link utility).

## Physical Port Mapping

| Port | Device |
|------|--------|
| 1 | Router (internet uplink) |
| 2 | Workstation |
| 3 | alpha.local |
| 4 | beta.local |
| 5 | Raspberry Pi (QDevice) |

## 802.1Q VLAN Configuration

### VLAN 10 â Management
| Port | Member Type |
|------|-------------|
| 1 | Untagged |
| 2 | Untagged |
| 3 | Untagged |
| 4 | Untagged |
| 5 | Untagged |

### VLAN 20 â Lab
| Port | Member Type |
|------|-------------|
| 3 | Tagged |
| 4 | Tagged |
| All others | Not Member |

### VLAN 99 â Heartbeat
| Port | Member Type |
|------|-------------|
| 3 | Tagged |
| 4 | Tagged |
| All others | Not Member |

## PVID Settings

| Port | PVID |
|------|------|
| 1 | 10 |
| 2 | 10 |
| 3 | 10 |
| 4 | 10 |
| 5 | 10 |

> PVID determines which VLAN untagged traffic belongs to. All ports use VLAN 10 as default since management traffic is untagged.

## Why Tagged vs Untagged?

- **Untagged**: The device does not know about VLANs (router, workstation, RPi). Switch handles VLAN assignment transparently.
- **Tagged**: The device (Proxmox) understands VLAN tags and sorts traffic to the correct bridge (vmbr0, vmbr20, vmbr99).
