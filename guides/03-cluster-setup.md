# Setting up my Proxmox cluster

## Step 1 – Create the Proxmox Cluster

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

## Step 2 – Join beta to the Cluster

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


