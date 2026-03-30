# CLAUDE.md — Proxmox Homelab

GitHub repo: https://github.com/Devin-McLellan/proxmox-lab

Two-node Proxmox VE HA-cluster i hemmet. Målet är att lära sig Linux, Ansible, Windows Server-administration och praktisk cybersäkerhet.

## Infrastruktur

| Enhet | IP | Roll |
|---|---|---|
| alpha.local | 192.168.1.71 | Proxmox nod 1 |
| beta.local | 192.168.1.70 | Proxmox nod 2 |
| Raspberry Pi | 192.168.1.3 | QDevice (quorum) |
| TP-Link TL-SG105E | 192.168.1.28 | Managed switch |

## Nätverk

| VLAN | Namn | Subnät | Proxmox-interface | Syfte |
|---|---|---|---|---|
| 10 | Management | 192.168.1.0/24 | vmbr0 | Proxmox-gränssnitt, dator, RPi |
| 20 | Lab | 192.168.10.0/24 | vmbr20 (nic0.20) | Alla VM:ar |
| 99 | Heartbeat | 192.168.99.0/24 | vmbr99 (nic0.99) | Corosync mellan noderna |

## VM-plan

| VM | ID | Nod | OS | IP | Roll |
|---|---|---|---|---|---|
| DC01 | 100 | alpha | WS 2025 Server Core | 192.168.10.10 | Primary Domain Controller |
| DC02 | 101 | beta | WS 2025 Server Core | 192.168.10.11 | Secondary Domain Controller |
| DHCP01 | 102 | alpha | - | 192.168.10.20 | Primary DHCP |
| DHCP02 | 103 | beta | - | 192.168.10.21 | Secondary DHCP |
| FS01 | 104 | alpha | - | 192.168.10.30 | File Server |
| WIN10 | 105 | alpha | Windows 10 | DHCP | Klientdator, domänansluten |
| Ubuntu | 106 | alpha | Ubuntu 24.04 | 192.168.10.50 | Docker, loggning |

## Kursinnehåll

1. [x] Access Proxmox Management Interface
2. [x] Lab Architecture & nätverksupplägg
3. [x] VLAN-konfiguration (switch + Proxmox)
4. [x] Corosync heartbeat på VLAN 99
5. [ ] Installera Windows Server 2025 Server Core + Active Directory (DC01, DC02)
6. [ ] Service Accounts, SPNs, Network Shares
7. [ ] Installera Windows 10 + domänanslutning
8. [ ] Third Party Apps + Windows Server 2022
9. [ ] Konfigurera ADCS (Active Directory Certificate Services)
10. [ ] ESC1-sårbarhet med Certify.exe (attack & försvar)
11. [ ] Ubuntu 24.04 + Docker
12. [ ] Web App med Docker Compose
13. [ ] Centraliserad loggning (Elasticsearch, Kibana)

## Mentorsstil
- Ta ett steg i taget, vänta på bekräftelse
- Förklara "varför" bakom varje steg
- Introducera Ansible där det passar
- Säkerhetsperspektiv genomgående
- Kör som i verkligheten (Server Core, VirtIO, HA)

## Lokala filer (ej i git)

Lokala filer och konfiger lagras i `D:\Proxmox\` (pushas ej till GitHub):

| Mapp | Innehåll |
|------|----------|
| `D:\Proxmox\Configs\` | WireGuard-konfar, nätverkskonfar |
| `D:\Proxmox\ISO\` | OS-images |
| `D:\Proxmox\Keys\` | Lokala nycklar och tokens (se Bitwarden) |
| `D:\Proxmox\QA\` | Testanteckningar |
| `D:\Proxmox\Scripts\` | Lokala hjälpscript |

Changelog skrivs direkt i `~/proxmox-lab/CHANGELOG.md` (inte i D:\Proxmox\).

## Sessionslogg

### 2026-03-30
- WireGuard VPN installerat på alpha.local (wg0, UDP 51820, VPN-subnet 10.10.10.0/24)
- DuckDNS konfigurerat: `kevins-proxmox.duckdns.org`, cronjob var 5:e minut
- Mac-klient verifierad fungera via mobilhotspot; Windows-klient ej testat externt (hairpin NAT)
- CHANGELOG.md skapad och pushad till GitHub
- DC01 VM-specs flyttade från configs/ till vms/DC01.md
- guides/02-vlan-switch.md sammanslagen (remote + lokal version)
