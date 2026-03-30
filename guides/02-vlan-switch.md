# Guide 02 - VLAN Switch Configuration (TP-Link TL-SG105E)

Step by step documentation from when I configured the TP-Link TL-SG105E switch for the Proxmox cluster.

## Configuration Tool

Configure using **TP-Link Easy Smart Configuration** (TP-Link utility) via `http://192.168.1.28`.

## Cable Layout / Physical Port Mapping

| Port | Device |
|------|--------|
| 1 | Router (internet uplink) |
| 2 | Workstation |
| 3 | alpha.local |
| 4 | beta.local |
| 5 | Raspberry Pi (QDevice) |

## 802.1Q VLAN Configuration

Two VLANs configured to separate management traffic from cluster heartbeat traffic.

**Why VLAN 99 is isolated:** Corosync heartbeat must be separated from LAN to prevent false split-brain scenarios in the Proxmox cluster.

### VLAN 10 - Management
| Port | Member Type |
|------|-------------|
| 1 | Untagged |
| 2 | Untagged |
| 3 | Untagged |
| 4 | Untagged |
| 5 | Untagged |

### VLAN 20 - Lab
| Port | Member Type |
|------|-------------|
| 3 | Tagged |
| 4 | Tagged |
| All others | Not Member |

### VLAN 99 - Heartbeat
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

## Step-by-Step Configuration

1. Open `http://192.168.1.28` in a browser
2. Navigate to **VLAN → 802.1Q VLAN**
3. Create VLAN 10 (Management) – ports 1–5 untagged
4. Create VLAN 20 (Lab) – ports 3–4 tagged
5. Create VLAN 99 (Heartbeat) – ports 3–4 tagged
6. Navigate to **VLAN → 802.1Q PVID Setting**
7. Set PVID 10 on ports 1–5

## Why Tagged vs Untagged?

- **Untagged**: The device does not know about VLANs (router, workstation, RPi). Switch handles VLAN assignment transparently.
- **Tagged**: The device (Proxmox) understands VLAN tags and sorts traffic to the correct bridge (vmbr0, vmbr20, vmbr99).
