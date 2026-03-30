# Setup Guide

Step-by-step documentation of the home lab build process.

---

## Step 1 – Install Proxmox VE

Install Proxmox VE on both nodes from the official ISO.

- **alpha.local** → IP: `192.168.1.71`
- **beta.local** → IP: `192.168.1.70`

Access the web UI at `https://<node-ip>:8006`

---

## Step 2 – Running Proxmox VE helperscript

To prevent nagging pop-ups when activating Proxmox, I ran the following POST-install script on both of my Proxmox nodes: 

bash -c "$(curl -fsSL https://raw.githubusercontent.com/community-scripts/ProxmoxVE/main/tools/pve/post-pve-install.sh)"

Verifying with **apt update**

---