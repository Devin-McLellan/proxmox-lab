# Session 01 — Network Foundation & OPNsense

**Datum:** 2026-04-11
**Tid:** Kväll
**Fokus:** Nätverksdesign, VLAN-dokumentation, OPNsense-installation

---

## Mål

- Dokumentera nätverksarkitekturen (VLAN 10/20/99)
- Installera och konfigurera OPNsense som router/firewall för lab-VMs
- Verifiera att lab-VMs kan nå internet via OPNsense

---

## Genomfört

### Nätverksdokumentation

- Dokumenterat VLAN-design: VLAN 10 (Management), VLAN 20 (Lab), VLAN 99 (Heartbeat)
- Dokumenterat TP-Link TL-SG105E portkonfiguration
- Dokumenterat Proxmox nätverksgränssnitt per nod (vmbr0/20/99)

### OPNsense VM (ID: 200)

- Skapat VM på alpha med specifikationer: 2c/2GB/16GB, SeaBIOS, i440fx
- net0 (WAN): vmbr0 — kopplad till fysiska routern
- net1 (LAN): vmbr20 — gateway för VLAN 20
- Installerat OPNsense 26.1.2 via konsol (ZFS, Stripe, da0)
- Tilldelat interfaces: WAN=vtnet0, LAN=vtnet1
- Konfigurerat LAN-IP: 10.20.0.1/24
- Aktiverat DHCP-pool: 10.20.0.100–200
- Slutfört Web GUI Wizard (hostname: OPNsense, domain: lab.local)

### OPNsense WAN

- WAN fick IP via DHCP: **192.168.1.32**
- Felsökning krävdes för att nå GUI från VLAN 10 (`pfctl -d`, temporär route)

---

## Problem & Lösningar

| Problem | Orsak | Lösning |
|---------|-------|---------|
| Kunde inte nå GUI på 10.20.0.1 från VLAN 10 | Firewall blockerade cross-VLAN trafik | `pfctl -d` + route add på Windows-PC |
| Block RFC1918 blockerade trafik från LAN | OPNsense-standard | Avmarkera "Block RFC1918" i wizard |

---

## Lärdomar

- OPNsense måste installeras INNAN domain controllers — annars saknar VMs DHCP och gateway
- `pfctl -d` stänger brandväggen temporärt för felsökning (glöm inte slå på igen med `pfctl -e`)
- VirtIO NIC fungerar för OPNsense (FreeBSD), men INTE för Windows VMs — använd E1000 för Windows

---

## Nästa steg

- [ ] Reservera statisk WAN-IP (192.168.1.30) i routern
- [ ] Konfigurera permanent firewall-regel: VLAN 10 → VLAN 20 (management access)
- [ ] Slutföra DC01 — AD DS-installation och konfiguration
- [ ] Fortsätta kursmoment 5: Windows Server 2025 & Active Directory
