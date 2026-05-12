# Client01 - Management Workstation

## Overview

Windows 11 workstation used for lab management and personal projects.
Connected to VLAN 20 — accesses other lab VMs via OPNsense routing.

## VM Specifications

| Setting       | Value                          |
|---------------|--------------------------------|
| VM ID         | 103                            |
| Node          | alpha.local (192.168.1.71)     |
| OS            | Windows 11 25H2 (Swedish)      |
| BIOS          | UEFI (OVMF) + Secure Boot      |
| CPU           | 2 cores (x86-64-v2-AES)        |
| Memory        | 8192 MB                        |
| Disk          | 64 GiB (SATA, local-lvm)       |
| TPM           | v2.0                           |
| Network       | vmbr20, E1000, VLAN tag 20     |
| IP Address    | 10.20.0.x (DHCP from OPNsense) |
| Gateway       | 10.20.0.1 (OPNsense)           |

## Purpose

- **Lab management** — access DC01, DC02, file servers from within VLAN 20
- **Personal projects** — development workstation for tools, scripts, and labs

## Status

| Task                          | Status      |
|-------------------------------|-------------|
| VM created in Proxmox         | ✅ Done     |
| Windows 11 installed          | ⏳ Pending  |
| VirtIO drivers installed      | ⏳ Pending  |
| Domain joined (lab.local)     | ⏳ Pending  |

## Notes

- VirtIO drivers ISO mounted (virtio-win-0.1.285) — install after Windows setup
- UEFI + TPM 2.0 required for Windows 11
