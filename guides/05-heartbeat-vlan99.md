# Guide 05 - Heartbeat Network (VLAN 99 / Corosync)

## Overview

By default, Corosync uses the management network (VLAN 10) for cluster heartbeat traffic. This guide moves heartbeat to a dedicated VLAN 99 to isolate cluster communication from management traffic.

**Why separate heartbeat traffic?**
- Prevents false split-brain scenarios caused by management network congestion
- Mirrors production cluster design
- Dedicated, reliable path for cluster health checks

## Step 1 - Create Network Interfaces (both nodes)

On each Proxmox node via **System â Network**:

### Create Linux VLAN interface
- Name: 
- VLAN raw device: 
- VLAN Tag: 

### Create Linux Bridge
- Name: 
- Bridge ports: 
- IPv4/CIDR:  (alpha) or  (beta)
- Autostart: enabled
- Comment: 

Click **Apply Configuration** on both nodes.

### Verify connectivity
```bash
# On alpha:
ping 192.168.99.70
```

## Step 2 - Update Corosync Configuration

On **alpha**, edit the config:
```bash
nano /etc/corosync/corosync.conf
```

Change  for both nodes:
```
ring0_addr: 192.168.99.71   # alpha
ring0_addr: 192.168.99.70   # beta
```

Sync to beta and restart:
```bash
scp /etc/corosync/corosync.conf root@192.168.1.70:/etc/corosync/corosync.conf
systemctl restart corosync
```

On **beta**:
```bash
systemctl restart corosync
```

## Step 3 â Verify

```bash
pvecm status
```

Expected output shows nodes using  addresses with 3/3 votes and .

## Also create Lab network interfaces (VLAN 20)

While configuring network interfaces, also create on both nodes:

### Linux VLAN
- Name: 
- VLAN raw device:   
- VLAN Tag: 

### Linux Bridge
- Name: 
- Bridge ports: 
- IPv4/CIDR: leave empty (VMs manage their own IPs)
- Autostart: enabled
- Comment: 
