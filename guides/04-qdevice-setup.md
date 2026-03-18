# 04 - QDevice Setup (Raspberry Pi)

## Overview

A two-node cluster has no built-in tiebreaker — if one node goes down, the remaining node loses quorum and shuts down VMs to prevent split-brain. A QDevice adds a third vote so quorum is maintained with just one node online.

| Device      | IP Address   | Role          |
|-------------|--------------|---------------|
| alpha.local | 192.168.1.71 | Proxmox node  |
| beta.local  | 192.168.1.70 | Proxmox node  |
| raspberrypi | 192.168.1.3  | QDevice voter |

Votes: alpha (1) + beta (1) + QDevice (1) = 3 total, quorum = 2

---

## Step 1 – Prepare the Raspberry Pi

SSH into the Raspberry Pi:

```bash
ssh pi@192.168.1.3
```

Enable root SSH login:

```bash
sudo passwd root
sudo nano /etc/ssh/sshd_config
# Set: PermitRootLogin yes
sudo systemctl restart ssh
```

Install the QDevice daemon:

```bash
sudo apt update && sudo apt install corosync-qnetd -y
```

---

## Step 2 – Prepare Both Proxmox Nodes

Run on **both alpha and beta**:

```bash
apt install corosync-qdevice -y
```

---

## Step 3 – Configure QDevice from Alpha

Run on **alpha.local**:

```bash
pvecm qdevice setup 192.168.1.3
```

This will SSH into the Raspberry Pi and configure the QDevice automatically.

---

## Step 4 – Verify

Run on either Proxmox node:

```bash
pvecm status
```

Expected output:

```
Quorum information
------------------
Date:             ...
Quorum provider:  corosync_votequorum
Nodes:            2
Node ID:          0x00000001
Ring ID:          ...
Quorate:          Yes

Votequorum information
----------------------
Expected votes:   3
Highest expected: 3
Total votes:      3
Quorum:           2
Flags:            Quorate Qdevice

Membership information
----------------------
Nodeid    Votes  Qdevice  Name
       1      1    A,V,NMW  alpha.local (local)
       2      1    A,V,NMW  beta.local
       0      1            Qdevice
```

---

## Notes

- The QDevice does not run any VMs — it only votes for quorum
- If the RPi goes offline, the two Proxmox nodes still have quorum (2/3 votes)
- If one Proxmox node goes offline, the remaining node + QDevice still have quorum (2/3 votes)
