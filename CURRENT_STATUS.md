# CURRENT_STATUS.md

> Genererad: 2026-05-22

---

## 1. Filstruktur

```
proxmox-lab/
├── CHANGELOG.md
├── CLAUDE.md
├── CURRENT_STATUS.md          <- denna fil
├── README.md
├── ansible/
│   ├── ansible.cfg
│   ├── inventory/
│   │   ├── group_vars/
│   │   │   ├── all.yml
│   │   │   ├── proxmox.yml
│   │   │   └── windows.yml
│   │   └── hosts.yml
│   └── playbooks/
│       ├── dc.yml
│       ├── proxmox.yml
│       └── site.yml
├── configs/
│   └── DC01/
│       ├── README.md
│       └── config.ps
├── guides/
│   ├── 01-proxmox-setup.md
│   ├── 02-vlan-switch.md
│   ├── 03-cluster-setup.md
│   ├── 04-qdevice-setup.md
│   ├── 05-heartbeat-vlan99.md
│   ├── 06-ansible-setup.md
│   ├── 07-dc01-setup.md
│   ├── opnsense-setup.md
│   └── sessions/
│       ├── README.md
│       ├── 2026-04-11-network-opnsense.md
│       └── 2026-05-12-infrastructure-review.md
├── journal/
│   └── 2026-04-09.md
├── network/
│   ├── network-plan.md
│   └── vlan-design.md
├── security/
│   ├── firewall-rules.md
│   └── hardening.md
└── vms/
    ├── Client01.md
    ├── DC01.md
    ├── opnsense.md
    └── vm-plan.md
```

---

## 2. docker-compose.yml

Filen **existerar inte annu** i repot.

Den ar planerad som en del av OWASP LLM Top 10-labbet (steg 10-11 i curriculum):

- Tjanster som ska definieras: **Ollama**, **ChromaDB**, **LangChain-app**, **Wazuh-agent**
- Deployment-target: Ubuntu 24.04 VM (VLAN 20, 10.20.0.x) bakom OPNsense + Suricata
- Forutsatter att OPNsense, DC01 och grundinfrastruktur ar klar

---

## 3. Modulstatus: Python-komponenter

Filerna **existerar inte annu** i repot. Dessa ar planerade moduler for AI Security-labbet.

| Modul | Status | Beskrivning |
|-------|--------|-------------|
| `alert_engine.py` | Ej paborjad | Tar emot Wazuh/Suricata-alerts och triggar automatiserade svar;ingar i OWASP LLM-labb |
| `ollama_client.py` | Ej paborjad | Wrapper mot lokal Ollama-instans (LLM01-LLM10 angreppssimuleringar och prompt injection-tester) |
| `gui.py` | Ej paborjad | Grafiskt granssnittt (troligen Tkinter eller Streamlit) for att visa alert-floden och LLM-interaktioner i realtid |

**Beroenden for att kunna starta:**
1. OPNsense - statisk WAN-IP + brandvaggsregler (pagaende, nasta steg)
2. DC01/DC02 - Active Directory uppe och kor
3. Ubuntu 24.04 VM med Docker Compose
4. Wazuh SIEM installerat och log shipper konfigurerad

---

## Nuvarande steg i curriculum

```
[x] 1. Proxmox Management Interface
[x] 2. Labbarkitektur & natverksdesign
[x] 3. ISO-hantering
[ ] 4. OPNsense - brandvaggsregler + DHCP-verifiering  <- AKTIV
[ ] 5. Windows Server 2025 & Active Directory (DC01)
[ ] 6. Service Accounts, SPNs, natverksdelar
[ ] 7. Windows 10 & domananslutnin
[ ] 8. Tredjepartsappar & Windows Server 2022
[ ] 9. ADCS + ESC1 (sarbart certifikatmall)
[ ] 10. Ubuntu 24.04 & Docker
[ ] 11. Webbapp via Docker Compose
[ ] 12. Elasticsearch, Kibana & log shipping
```
