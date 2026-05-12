# Session 02 — Infrastructure Review & OPNsense WAN

**Datum:** 2026-05-12
**Fokus:** Genomgång av hela infrastrukturen, dokumentationsuppdatering, OPNsense statisk WAN-IP

---

## Mål

- Få full kontroll och förståelse över befintlig infrastruktur
- Rätta inkonsistenser i dokumentationen
- Sätta statisk WAN-IP för OPNsense

---

## Genomfört

### Infrastrukturgenomgång

- Installerade GitHub CLI (gh) på alpha för repo-access
- Klonade privat repo (proxmox-lab) till alpha
- Genomgick hela infrastrukturen: switch, Proxmox bridges, OPNsense, VMs
- Verifierade klusterstatus: 3/3 röster, båda noder online, heartbeat fungerar

### Nätverksförståelse

- Gick igenom switch-design: VLAN 10 (management), VLAN 20 (lab), VLAN 99 (heartbeat)
- Gick igenom Proxmox nätverkslager: nic0 → nic0.20/99 → vmbr0/20/99
- Förstod varför OPNsense inte kan hantera management eller heartbeat (chicken-and-egg)
- Förstod hairpin-routing: lab-VM → switch → OPNsense → switch → router

### Dokumentationsuppdateringar

- Rättade VLAN 99 subnet: `10.99.0.0/24` → `192.168.99.0/24` (matchar faktisk config)
- Rättade VLAN 20 subnet i network-plan.md: `192.168.10.0/24` → `10.20.0.0/24`
- Rättade DC01 nätverk: `vmbr0` → `vmbr20`, IP `192.168.1.10` → `10.20.0.10`
- Uppdaterade progress och current step i README och CLAUDE.md
- Lade till samarbetsmodell i CLAUDE.md: Kevin äger projektet, Claude är mentor
- Dokumenterade Client01 (VM 103) — Windows 11 25H2, management/personliga projekt

### OPNsense WAN

- Satte statisk DHCP-reservation i routern för OPNsense WAN: **192.168.1.32**
- Plan: sätt statisk IP direkt på OPNsense WAN-interfacet när router byts ut

---

## Lärdomar

- VLAN 99 (heartbeat) och VLAN 10 (management) får aldrig gå via OPNsense — noden måste vara åtkomlig oavsett VM-status
- nic0.20 kopplas automatiskt till nic0 via namngivningskonventionen — ingen manuell config krävs
- En bridge (vmbr) är en virtuell switch inuti Proxmox — VMs kopplar sin NIC till bridgen precis som en kabel till en fysisk switch
- Statisk IP direkt på OPNsense WAN är bättre än DHCP-reservation — överlever routerbyte

---

## Nästa steg

- [ ] Konfigurera firewall-regel: VLAN 10 → VLAN 20 (management access)
- [ ] Verifiera att Client01 och DC01 får DHCP från OPNsense (10.20.0.100–200)
- [ ] Slutföra DC01 — AD DS + DNS installation, promota till domain controller (lab.local)
