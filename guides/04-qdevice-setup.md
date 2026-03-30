# Setting up my Raspberry Pi as QDevice

## Step 1 – Configure Raspberry Pi as QDevice

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