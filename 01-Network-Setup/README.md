# Network Infrastructure and Virtualization Setup

## Overview

This section documents the virtualization infrastructure and network configuration developed for my Ultimate Cybersecurity Home Lab.

The environment was built using VMware Workstation Pro and Proxmox VE to host multiple virtual machines for network security, Linux administration, firewall management, and cybersecurity testing.

## Host System Specifications

| Component | Configuration |
|---|---|
| Host Operating System | Windows |
| Processor | Intel Core i9-10900K |
| CPU | 10 Cores / 20 Threads |
| Memory | 32 GB RAM |
| Virtualization Software | VMware Workstation Pro 26H1 |
| Hypervisor | Proxmox VE 9.2.2 |

## Virtualization Architecture

I installed VMware Workstation Pro on my Windows computer and deployed Proxmox VE as a nested virtual machine.

This allows Proxmox to manage multiple virtual machines without requiring a separate physical server.

### Proxmox Configuration

| Resource | Configuration |
|---|---|
| Virtual CPUs | 4 |
| Allocated Memory | 16 GB |
| Virtual Disk | 200 GB |
| Network Connection | NAT |
| Nested Virtualization | Enabled |
| Management IP | 192.168.10.10/24 |
| Gateway | 192.168.10.2 |

### Virtual Machines

I created three primary virtual machines inside Proxmox.

| VM ID | Name | Purpose |
|---|---|---|
| 201 | prod-fw | pfSense Firewall |
| 202 | prod-Kali | Kali Linux Security Testing |
| 203 | prod-docker | Ubuntu Server, Docker, and Portainer |

Each virtual machine serves a specific purpose within the cybersecurity lab.

## Virtual Network Configuration

I configured two Linux network bridges within Proxmox.

### vmbr0 — Management Network

- **Type:** Linux Bridge
- **IP Address:** 192.168.10.10/24
- **Gateway:** 192.168.10.2
- **Physical Interface:** nic0
- **VLAN Aware:** No
- **Status:** Active

This bridge provides network connectivity for Proxmox and the external side of the pfSense firewall through VMware NAT.

### vmbr1 — Internal Lab Network

- **Type:** Linux Bridge
- **Configured Address:** 10.10.1.0/24
- **Physical Interface:** None
- **VLAN Aware:** Yes
- **Status:** Active
- **Purpose:** Internal cybersecurity lab networking

This bridge supports virtual networking and VLAN-tagged traffic within the lab.

**Configuration note:** The address 10.10.1.0/24 appears in the bridge configuration and needs verification because it represents the subnet address rather than a typical usable host address.

### pfSense Network Interfaces

| Interface | Address | Purpose |
|---|---|---|
| WAN | 192.168.10.129/24 | External connectivity |
| LAN | 10.10.1.254/24 | Internal lab gateway |

I also configured VLAN-aware networking to support multiple logical networks within the lab.

Detailed VLAN assignments and firewall rules are documented separately.

## Troubleshooting and Problem Solving

### Hardware Virtualization

**Issue:** Hardware virtualization was initially disabled on the Windows host.

**Resolution:** Enabled Intel virtualization support in the BIOS and configured VMware to expose hardware-assisted virtualization to Proxmox.

### VMware Virtual Disk Error

**Issue:** VMware reported an internal virtual disk consistency error during the initial startup.

**Resolution:** Used VMware's disk repair functionality and successfully booted Proxmox.

### Proxmox Package Repository Errors

**Issue:** Package updates failed with HTTP 401 Unauthorized errors because the Enterprise repositories required a subscription.

**Resolution:**
- Disabled the Proxmox Enterprise repository.
- Disabled the Ceph Enterprise repository.
- Enabled the Proxmox No-Subscription repository.
- Verified successful package database refresh.

## Results

Successfully deployed a nested virtualization environment capable of running multiple cybersecurity virtual machines.

Configured virtual networking to support firewall administration, Linux systems, and VLAN-based network segmentation.

Established a working foundation for future security monitoring, vulnerability assessments, and controlled network testing.

## Technical Skills Demonstrated

- Hypervisor Administration
- Nested Virtualization
- Virtual Machine Deployment
- Linux Network Bridges
- VLAN-Aware Networking
- IP Addressing and Subnetting
- Network Troubleshooting
- Package Repository Management
- Infrastructure Documentation

## Supporting Evidence

Screenshots collected for this section:

- `proxmox-server-summary.png`
- `proxmox-network-bridges.png`
- `proxmox-virtual-machines.png`

Screenshots will be uploaded and linked as supporting evidence after review.

## Future Improvements

- Verify the internal bridge IP configuration.
- Validate VLAN routing and network isolation.
- Document resource utilization under load.
- Conduct connectivity and firewall policy tests.
