# 03 - Proxmox Cluster Setup

## Overview

Creating a two-node Proxmox cluster using alpha and beta as nodes, with a Raspberry Pi as QDevice for quorum.

| Node        | IP Address    | Role         |
|-------------|---------------|--------------|
| alpha.local | 192.168.1.71  | Cluster init |
| beta.local  | 192.168.1.70  | Join node    |
| RPi QDevice | 192.168.1.3   | Quorum voter |

---

## Step 1 – Create the Cluster on Alpha

SSH into alpha or open the Proxmox shell:

```bash
pvecm create my-cluster
```

Verify that the cluster was created:

```bash
pvecm status
# Expected: Nodes: 1, Quorate: Yes
```

---

## Step 2 – Join Beta to the Cluster

Open the shell on **beta.local** and run:

```bash
pvecm add 192.168.1.71
```

> If beta has existing VMs blocking the join, add the `--force` flag.

Verify on alpha:

```bash
pvecm nodes
# Expected: both alpha and beta listed
```

---

## Step 3 – Verify Cluster Status

Run on either node:

```bash
pvecm status
```

Expected output:

```
Cluster information
-------------------
Name:             my-cluster
Config Version:   2
Transport:        knet
Secure auth:      on

Quorum information
------------------
Nodes:     2
Quorate:   Yes
```

---

## Notes

- Both nodes must be reachable on the network before running `pvecm add`
- Cluster traffic runs over VLAN 99 (Heartbeat) on ports 3–4 of the switch
- QDevice setup is documented in `04-qdevice-setup.md`
