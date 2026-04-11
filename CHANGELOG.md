# Changelog — Proxmox Home Lab

## 2026-04-11 — Network Foundation & OPNsense

### VLAN Architecture

- Dokumenterat VLAN-design: VLAN 10 (Management), VLAN 20 (Lab), VLAN 99 (Heartbeat)
- Dokumenterat TP-Link TL-SG105E portkonfiguration (trunk-portar för alpha/beta, untagged för workstation/RPi)
- Verifierat Proxmox nätverksgränssnitt: vmbr0/20/99 aktiva på båda noder

### OPNsense VM (ID: 200)

**Mål:** Installera router/firewall för VLAN 20 — nödvändig innan domain controllers kan konfigureras.

**Vad som gjordes:**

- Skapat VM på alpha: 2c/2GB/16GB, SeaBIOS, i440fx
  - net0 (WAN): vmbr0 — kopplad till fysiska routern
  - net1 (LAN): vmbr20 — gateway för VLAN 20
- Installerat OPNsense 26.1.2 via konsol (ZFS, Stripe, da0)
- Tilldelat interfaces: WAN=vtnet0, LAN=vtnet1
- Konfigurerat LAN-IP: `10.20.0.1/24`
- Aktiverat DHCP-pool: `10.20.0.100–200`
- Slutfört Web GUI Wizard:
  - Hostname: OPNsense, Domain: lab.local, Timezone: Europe/Stockholm
  - Block RFC1918: avmarkerat (krävs för privat LAN-subnät)

**WAN:** DHCP från router → `192.168.1.32`

**Felsökning:**

| Problem | Lösning |
|---|---|
| Kunde inte nå GUI på 10.20.0.1 från VLAN 10 | `pfctl -d` + `route add 10.20.0.0 mask 255.255.255.0 192.168.1.32` på Windows |
| Block RFC1918 blockerade trafik | Avmarkerat i wizard |

### Repo-uppdateringar

- `network/vlan-design.md` — VLAN-design och switch-portkonfiguration
- `guides/opnsense-setup.md` — Installationsguide för OPNsense
- `guides/sessions/` — Ny katalogstruktur för kvällssessioner
- `guides/sessions/2026-04-11-network-opnsense.md` — Session 01-logg
- `vms/opnsense.md` — VM-spec för OPNsense (VM 200)
- `CLAUDE.md` — Uppdaterad med fullständig projektkontext

### Nästa steg

- [ ] Reservera statisk WAN-IP (192.168.1.30) i routern
- [ ] Konfigurera permanent firewall-regel: VLAN 10 → VLAN 20
- [ ] Slutföra DC01 — AD DS-installation och konfiguration
- [ ] Kursmoment 5: Windows Server 2025 & Active Directory

---

## 2026-03-30 — WireGuard VPN & DuckDNS Setup

### WireGuard VPN

**Goal:** Enable secure remote access to the Proxmox cluster from anywhere.

**What was done:**

- Installed WireGuard on `alpha.local` (192.168.1.71)
- Generated key pairs for server and two clients (Windows PC + Mac)
- Configured WireGuard server interface `wg0`:
  - VPN subnet: `10.10.10.0/24`
  - Server VPN IP: `10.10.10.1`
  - Listening port: UDP 51820
  - IP forwarding enabled via `sysctl`
  - iptables NAT rules for routing VPN traffic into `vmbr0`
- Enabled and started `wg-quick@wg0` as a systemd service (autostart on boot)
- Configured two client peers:
  - Windows PC → `10.10.10.2/32`
  - Mac → `10.10.10.3/32`
- Split tunnel configured — only `192.168.1.0/24` and `10.10.10.0/24` routed via VPN

**Client configs:**

| Enhet       | VPN IP       | Konfig-fil                  |
|-------------|-------------|------------------------------|
| Windows PC  | 10.10.10.2  | windows-proxmox-lab.conf     |
| Mac         | 10.10.10.3  | mac-proxmox-lab.conf         |

**Port forward:**

- Router: Inseego (Tele2 4G/5G)
- Regel: UDP 51820 → 192.168.1.71

**Notering:** WireGuard på Windows testades ej från externt nät pga hairpin NAT-begränsning på routern. Mac-klienten verifierades fungera från mobilhotspot.

---

### DuckDNS — Dynamisk DNS

**Goal:** Fast domännamn som alltid pekar på hemlanätets publika IP, oavsett om IP:n ändras.

**Vad som gjordes:**

- Konto skapat på [duckdns.org](https://www.duckdns.org)
- Domän registrerad: `kevins-proxmox.duckdns.org`
- Uppdateringsscript skapat på alpha: `/etc/duckdns/duck.sh`
  - Anropar DuckDNS API med token
  - IP detekteras automatiskt (ingen hårdkodad IP)
- Cronjob konfigurerat för root: körs var 5:e minut
- Mac WireGuard-klient uppdaterad: `Endpoint = kevins-proxmox.duckdns.org:51820`

**Filer på alpha:**

| Fil                        | Syfte                              |
|----------------------------|------------------------------------|
| `/etc/wireguard/wg0.conf`  | WireGuard serverkonfiguration      |
| `/etc/duckdns/duck.sh`     | DuckDNS uppdateringsscript         |
| `/etc/duckdns/duck.log`    | Logg (OK = lyckad uppdatering)     |

---

### Nyckelhantering

Alla nycklar och token sparade i **Bitwarden** under mappen `Proxmox Lab`:
- Server Private Key
- Server Public Key
- Windows Client Private/Public Key
- Mac Client Private/Public Key
- DuckDNS Token

---

### Arkitektur efter detta steg

```
[Mac / Windows på distans]
         |
         | UDP 51820 (WireGuard, krypterat)
         v
kevins-proxmox.duckdns.org → [Inseego Router] → Port forward
         |
         v
   alpha.local (192.168.1.71) — wg0: 10.10.10.1
         |
    192.168.1.0/24
         |
   ┌─────┴──────┐
   alpha       beta
 (.71)        (.70)
```

---

### Nästa steg

- [ ] Testa Windows WireGuard-klient från externt nät (t.ex. mobilhotspot)
- [ ] Lab Architecture — VLAN-design i Proxmox
- [ ] Fortsätta kursinnehåll: ISO-uploads, Windows Server 2025, Active Directory
