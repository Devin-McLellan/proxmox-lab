# Changelog — Proxmox Home Lab

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