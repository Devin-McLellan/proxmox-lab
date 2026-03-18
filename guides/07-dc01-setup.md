# 07 - Installing DC01

## Overview

First domain controller in the Proxmox lab environment. Runs Actice Directory Domain Service (AD DNS) for the lab domain

## VM Specifications

| Setting       | Value                  |
|---------------|------------------------|
| VM ID         | 100                    |
| Node          | alpha.local            |
| OS            | Windows Server 2025    |
| CPU           | 4 cores (x86-64-v2)   |
| Memory        | 4096 MB                |
| Disk          | 60 GiB (SATA, local-lvm) |
| Network       | vmbr0, E1000           |
| IP Address    | 192.168.1.x            |
| HA Managed    | Yes                    |

## Installation steps

### Step 1
- Downloading ISO from Windows website
- Uploading the ISO-file to Proxmox server Alpha
- Beginning the installation of the Domain Controller. 

### Step 2
- Selected the "Windows Server 2025 (Desktop Experience)
- Custom Installation, 60 Gb of vmemory
- Set Administrator password, saved in BitWarden

### Step 3 Install Proxmox Guest Agent
- Download and install virtio-win drivers
- Enable QEMU Guest Agent in Proxmox

### Steg 4 Configure Active Directory
- Rename PC with Rename-Computer -Newname "DC01" -Restart
- Change timezome: Set-TimeZome -Name "W. Europe Standard Time "
- Changing IP-adress: Get-Netadapter

'TODO'


- Installed AD DS role via server manager
- Promoted server to domain controller
- Domanname: lab.local
- DNS installed on DC01

## Notes

- Windows couldn't detect disk during install - changed bus from VirtIO to SATA
- VirtIO drivers to be installed port-setup for better performance
