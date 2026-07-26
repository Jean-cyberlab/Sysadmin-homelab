# Phase 1 — Network Setup & Static IP Configuration

## Overview
Configured a three-VM isolated network in VMware Workstation Pro using 
Host-only networking (VMnet1). Assigned static IP addresses to all three 
virtual machines and verified connectivity between them.

## Environment
| VM | OS | IP Address | Role |
|----|----|-----------|------|
| DC01 | Windows Server 2019 | 192.168.80.10 | Domain Controller |
| CLIENT01 | Windows 10 | 192.168.80.20 | Client Machine |
| TesterLab | Ubuntu 24.04 LTS | 192.168.80.30 | Linux Server |

## Network Configuration
- **Hypervisor:** VMware Workstation Pro
- **Network Type:** VMnet1 Host-only (isolated — no internet)
- **Subnet:** 192.168.80.0/24
- **Gateway:** 192.168.80.1

## Steps Completed

### 1. Renamed Virtual Machines
- Windows Server 2019 renamed to **DC01**
- Windows 10 renamed to **CLIENT01**

### 2. Configured Static IPs
**DC01 (Windows Server 2019):**
- IP: 192.168.80.10
- Subnet: 255.255.255.0
- Gateway: 192.168.80.1
- DNS: 127.0.0.1 (points to itself)

**CLIENT01 (Windows 10):**
- IP: 192.168.80.20
- Subnet: 255.255.255.0
- Gateway: 192.168.80.1
- DNS: 192.168.80.10 (points to DC01)

**TesterLab (Ubuntu 24.04):**
- IP: 192.168.80.30
- Subnet: 255.255.255.0
- Gateway: 192.168.80.1
- DNS: 192.168.80.10 (points to DC01)

### 3. Fixed VMnet1 Subnet Mismatch
Identified and resolved a subnet mismatch — VMnet1 was configured as 
192.168.10.0 instead of 192.168.80.0. Corrected in VMware Virtual 
Network Editor.

### 4. Disabled Windows Server Firewall
Disabled firewall on DC01 to allow ICMP ping traffic for lab testing:

### 5. Verified Connectivity
Successfully pinged all VMs from Ubuntu:
- Ubuntu → DC01: 4/4 packets received
- Ubuntu → CLIENT01: 4/4 packets received

## Troubleshooting Encountered
- VMnet1 subnet was 192.168.10.0 instead of 192.168.80.0 — fixed in 
Virtual Network Editor
- Ubuntu netplan gateway had typo (192.168.10.1 instead of 192.168.80.1) 
— fixed by editing /etc/netplan/01-network-manager-all.yaml
- Windows Server firewall blocking ICMP — resolved by disabling firewall

## Key Commands Used
```bash
# Ubuntu — check IP address
ip addr

# Ubuntu — ping test
ping -c 4 192.168.80.10

# Windows — check IP configuration
ipconfig /all

# Windows — disable firewall
netsh advfirewall set allprofiles state off
```

## What I Learned
- How to configure static IP addresses on Windows Server, Windows 10, 
and Ubuntu Linux
- How VMware virtual networking works (Host-only vs NAT vs Bridged)
- How to troubleshoot network connectivity using ping and ipconfig
- How subnet mismatches prevent VM communication
- How Windows Firewall blocks ICMP by default

## Screenshots
See the screenshots folder for documented evidence of each step.
