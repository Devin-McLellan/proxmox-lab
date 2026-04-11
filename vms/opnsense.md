# OPNsense - Router/Firewall

## Overview

Central router and firewall for the proxmox-lab environment.
Handles inter-VLAN routing, DHCP, NAT, and firewall rules for VLAN 20.

## VM Specifications

| Setting       | Value                          |
|---------------|--------------------------------|
| VM ID         | 200                            |
| Node          | alpha.local (192.168.1.71)     |
| OS            | OPNsense 26.1.2 (FreeBSD)      |
| CPU           | 2 cores                        |
| Memory        | 2048 MB                        |
| Disk          | 16 GiB (SATA, local-lvm)       |
| NIC 0 (WAN)   | vmbr0 — uplink to router       |
| NIC 1 (LAN)   | vmbr20 — VLAN 20 lab network   |

## Network

| Interface | Role            | IP                      |
|-----------|-----------------|-------------------------|
| WAN       | Physical router | DHCP (192.168.1.x)      |
| LAN       | VLAN 20 gateway | 10.20.0.1/24            |

## Status

Installing — OPNsense 26.1.2

## Planned Configuration

- WAN: DHCP from router (192.168.1.x)
- LAN: 10.20.0.1/24 — default gateway for all lab VMs
- DHCP server: 10.20.0.100–200 (VLAN 20)
- Firewall: VLAN 20 cannot reach VLAN 10 by default
- Suricata IDS/IPS: AI security phase
- Rate limiting: Ollama protection (OWASP LLM10)
