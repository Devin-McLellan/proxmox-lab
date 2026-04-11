# OPNsense Setup Guide

## VM-specifikationer

| Inställning      | Värde                         |
|------------------|-------------------------------|
| VM ID            | 200                           |
| Nod              | alpha                         |
| CPU              | 2 cores (1 socket)            |
| RAM              | 2048 MB                       |
| Disk             | 16 GB (local-lvm, SCSI)       |
| BIOS             | SeaBIOS                       |
| Machine          | Default (i440fx)              |
| net0 (WAN)       | vmbr0, VirtIO, ingen VLAN-tagg |
| net1 (LAN)       | vmbr20, VirtIO, ingen VLAN-tagg|
| Proxmox Firewall | Avaktiverad                   |

## Installation

1. Ladda upp OPNsense ISO till local (alpha) → ISO Images
2. Skapa VM med specifikationerna ovan
3. Starta VM och öppna Console
4. Logga in: `installer` / `opnsense`
5. Välj: Continue with default keymap → Install (ZFS) → Stripe → da0
6. Sätt root-lösenord (spara i Bitwarden → Proxmox Lab)
7. Välj: Complete Install

## Initialkonfiguration (konsol)

Efter installation, välj alternativ från menyn:

### 1) Assign interfaces

```
LAGGs: N
VLANs: N
WAN: vtnet0
LAN: vtnet1
Optional: (Enter — ingen)
Proceed: y
```

### 2) Set interface IP address → LAN (1)

```
DHCP: N
IP: 10.20.0.1
Subnet: 24
Gateway: (Enter — ingen)
IPv6: N
DHCP-server: y
Start: 10.20.0.100
End: 10.20.0.200
HTTPS: N (behåll HTTPS)
Certifikat: y
GUI defaults: y
```

## Web GUI Wizard

Nå GUI via: `https://10.20.0.1`

Om du inte når GUI från VLAN 10:

1. Öppna Shell (option 8) i OPNsense-konsolen
2. Kör: `pfctl -d` (stänger brandväggen temporärt)
3. Nå GUI via WAN-IP: `https://192.168.1.32`

Lägg till route på Windows-PC:

```powershell
route add 10.20.0.0 mask 255.255.255.0 192.168.1.32
```

### Wizard-inställningar

| Fält              | Värde               |
|-------------------|---------------------|
| Hostname          | OPNsense            |
| Domain            | lab.local           |
| Timezone          | Europe/Stockholm    |
| DNS               | 8.8.8.8             |
| WAN Type          | DHCP                |
| Block RFC1918     | Avmarkerad ← Viktigt|
| LAN IP            | 10.20.0.1/24        |

## Viktiga felsökningskommandon

```bash
# Visa alla interfaces
ifconfig

# Specifikt interface
ifconfig vtnet0

# Stäng av brandväggen temporärt (felsökning)
pfctl -d

# Slå på brandväggen igen
pfctl -e
```

## Nästa steg

- [ ] Sätt statisk WAN-IP (reservera 192.168.1.30 i routern)
- [ ] Konfigurera LAN-åtkomst från VLAN 10 permanent via firewall-regel
- [ ] Aktivera Suricata IDS (efter kursens grundkonfiguration)
- [ ] Konfigurera rate limiting mot Ollama (OWASP LLM10)
