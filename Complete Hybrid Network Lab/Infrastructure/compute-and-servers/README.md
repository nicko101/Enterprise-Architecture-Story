# Infrastructure Architecture: Virtualization and Compute Fabric

## Overview
This section documents the physical and virtualized foundation of the portfolio. The infrastructure utilizes a multi-node Proxmox VE cluster integrated with a GNS3 simulation environment to bridge the gap between physical hardware and software-defined networking (SDN).

---

## Technical Infrastructure Map

[![Infrastructure Layout](https://raw.githubusercontent.com/nicko101/Enterprise-Architecture-Portfolio/main/resources/slides/infranew.png)](https://raw.githubusercontent.com/nicko101/Enterprise-Architecture-Portfolio/main/resources/slides/infranew.png)
*Figure 1: Comprehensive map detailing the relationship between the physical compute layer, the virtualization hypervisor, and the simulated network fabric.*

---

## Core Infrastructure Components

### 1. Virtualization Fabric (Proxmox VE)
The environment is hosted on a Proxmox cluster, providing the high-availability compute required for enterprise services.
* **Hypervisor Management**: Centralized orchestration of Linux Containers (LXC) and Kernel-based Virtual Machines (KVM).
* **Software Defined Networking**: Implementation of Linux Bridges and Open vSwitch (OVS) to facilitate traffic flow between virtual and physical interfaces.
* **Storage Governance**: Utilization of ZFS for data integrity and high-performance I/O for virtual disk images.

### 2. Network Simulation (GNS3)
A GNS3 server is integrated directly into the virtualization layer to simulate complex multi-vendor topologies.
* **Multi-Vendor Integration**: Hosting of Cisco IOSv, ArubaOS-CX, and Palo Alto PAN-OS virtual appliances.
* **Cloud-Bridge**: Utilization of specialized GNS3 Cloud nodes to bridge simulated traffic into the physical network and the Azure VPN gateway.

### 3. Compute and Server Roles
The infrastructure hosts the essential identity and management services for the Zero Trust fabric:
* **Identity Hosts**: Windows Server instances providing Active Directory Domain Services (AD DS).
* **Policy Orchestration**: Deployment of Aruba ClearPass Policy Manager as a virtual appliance.
* **Management Gateways**: Linux-based jump-hosts for secure administrative access.

---

## Operational Standards

### High Availability
The infrastructure is designed with resiliency in mind, utilizing automated backups and snapshots to ensure rapid recovery during engineering tests.

### Security Hardening
* **Management Isolation**: All infrastructure management interfaces (Proxmox, GNS3, Switch Management) are isolated on a dedicated, non-routable VLAN.
* **Access Control**: Administrative access requires identity verification and is logged via the centralized security plane.

---

## Technical Validation

[Image of a server rack showing organized cabling and high-density compute nodes]

Functional success is verified by the seamless delivery of traffic from a simulated client in GNS3, through the Proxmox bridge, across the physical Palo Alto firewall, and into the Azure VNet.

---
[Return to Root README](../../README.md)
