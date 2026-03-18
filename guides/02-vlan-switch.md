# Setup Guide

Step by step documentation from when I installed my Network switch

## Step 1

Connect cables according to: 

TL-SG105E Switch (192.168.1.28)
    ├── Port 1 ──► Router          (192.168.1.1)
    ├── Port 2 ──► Workstation     (192.168.1.27)
    ├── Port 3 ──► alpha.local     (192.168.1.71)
    ├── Port 4 ──► beta.local      (192.168.1.70)
    └── Port 5 ──► RPi QDevice     (192.168.1.3)

---

** Using the tool Easy Smart Configuration when configurating TP-link switch.

```

**Using the following VLAN-config: 

VLAN: 10 (LAN) and 99 (HEARTBEATS)
VLAN 10: Member port 1-4, untagged ports 1-4
VLAN 99: Member port 3-4, tagged ports 3-4

```