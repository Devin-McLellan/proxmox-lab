# Setup Guide

Step-by-step documentation of the home lab build process.

---

## Step 1 – Install Proxmox VE

Install Proxmox VE on both nodes from the official ISO.

- **alpha.local** → IP: `192.168.1.71`
- **beta.local** → IP: `192.168.1.70`

Access the web UI at `https://<node-ip>:8006`

---

## Step 2 – Configure VLANs on TP-Link TL-SG105E

1. Open `http://192.168.1.28` in a browser
2. Navigate to **VLAN → 802.1Q VLAN**
3. Create VLAN 10 (LAN) – ports 1–4 untagged
4. Create VLAN 99 (Heartbeat) – ports 3–4 tagged
5. Navigate to **VLAN → 802.1Q PVID Setting**
6. Set PVID 10 on ports 1–4

---

## Step 3 – Create the Proxmox Cluster

SSH into alpha (or use the Proxmox shell):

```bash
pvecm create my-cluster
```

Verify:

```bash
pvecm status
# Expected: Nodes: 1, Quorate: Yes
```

---

## Step 4 – Join beta to the Cluster

Open the shell on **beta.local** and run:

```bash
pvecm add 192.168.1.71
```

> If beta has existing VMs blocking the join, use `--force` flag.

Verify on alpha:

```bash
pvecm nodes
# Expected: both alpha and beta listed
```

---

## Step 5 – Configure Raspberry Pi as QDevice

### On the Raspberry Pi

Enable root SSH login:

```bash
sudo passwd root
sudo nano /etc/ssh/sshd_config
# Set: PermitRootLogin yes
sudo systemctl restart ssh
```

Install the QDevice daemon:

```bash
sudo apt install corosync-qnetd -y
```

### On both Proxmox nodes

```bash
apt install corosync-qdevice -y
```

### On alpha – run setup

```bash
pvecm qdevice setup 192.168.1.3
```

### Verify

```bash
pvecm status
# Expected:
# Expected votes: 3
# Flags: Quorate Qdevice
```

---

## Result

```
Cluster: my-cluster
Nodes:   2 (alpha + beta)
QDevice: Raspberry Pi (192.168.1.3)
Votes:   3 total, quorum = 2
Status:  Quorate
```
