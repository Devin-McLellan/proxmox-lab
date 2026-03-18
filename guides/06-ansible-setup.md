# 06 - Ansible Setup

## Overview

Ansible is used to automate configuration of the Proxmox nodes and VMs in the lab. Instead of manually configuring each server, playbooks define the desired state and Ansible applies it.

---

## Step 1 – Install Ansible on Workstation

Run on the workstation (192.168.1.27):

```bash
sudo apt update && sudo apt install ansible -y
```

Verify:

```bash
ansible --version
```

For Windows hosts (DC01, DC02), also install the WinRM collection:

```bash
ansible-galaxy collection install ansible.windows
```

---

## Step 2 – Configure Inventory

The inventory file is located at `ansible/inventory/hosts.yml`.

```yaml
all:
  children:
    proxmox:
      hosts:
        alpha.local:
          ansible_host: 192.168.1.71
        beta.local:
          ansible_host: 192.168.1.70
    windows:
      hosts:
        DC01:
          ansible_host: 192.168.1.10
        DC02:
          ansible_host: 192.168.1.11
```

---

## Step 3 – Configure Group Variables

**`group_vars/proxmox.yml`** — settings for Proxmox nodes:

```yaml
ansible_user: root
ansible_ssh_private_key_file: ~/.ssh/id_rsa
```

**`group_vars/windows.yml`** — settings for Windows VMs:

```yaml
ansible_user: Administrator
ansible_password: "{{ vault_windows_password }}"
ansible_connection: winrm
ansible_winrm_transport: ntlm
ansible_port: 5985
```

**`group_vars/all.yml`** — variables shared across all hosts:

```yaml
lab_domain: lab.local
dns_server: 192.168.1.10
```

---

## Step 4 – Test Connectivity

Test Proxmox nodes (SSH):

```bash
ansible proxmox -m ping -i ansible/inventory/hosts.yml
```

Test Windows VMs (WinRM):

```bash
ansible windows -m win_ping -i ansible/inventory/hosts.yml
```

Expected output for each host:

```
alpha.local | SUCCESS => { "ping": "pong" }
beta.local  | SUCCESS => { "ping": "pong" }
```

---

## Step 5 – Run Playbooks

Run the full site playbook:

```bash
ansible-playbook ansible/inventory/playbooks/site.yml -i ansible/inventory/hosts.yml
```

Run only Proxmox configuration:

```bash
ansible-playbook ansible/inventory/playbooks/proxmox.yml -i ansible/inventory/hosts.yml
```

Run only DC configuration:

```bash
ansible-playbook ansible/inventory/playbooks/dc.yml -i ansible/inventory/hosts.yml
```

---

## Repository Structure

```
ansible/
├── ansible.cfg
└── inventory/
    ├── hosts.yml
    ├── group_vars/
    │   ├── all.yml
    │   ├── proxmox.yml
    │   └── windows.yml
    └── playbooks/
        ├── site.yml
        ├── proxmox.yml
        └── dc.yml
```

---

## Notes

- Use `ansible-vault` to encrypt sensitive variables like passwords
- SSH keys should be set up on Proxmox nodes before running playbooks
- WinRM must be enabled on Windows VMs — see DC01 setup guide
