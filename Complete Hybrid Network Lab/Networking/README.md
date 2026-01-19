# Network Infrastructure: Hybrid Connectivity Fabric

## Overview
This directory documents the core networking architecture for the environment. The design utilizes a hub-and-spoke model to bridge on-premises physical and virtualized infrastructure with a Microsoft Azure tenant, prioritizing secure tunneling, predictable path selection, and high-performance throughput.

---

## Technical Infrastructure Layout

[![Network Infrastructure Overview](https://raw.githubusercontent.com/nicko101/Enterprise-Architecture-Portfolio/main/resources/slides/infranew.png)](https://raw.githubusercontent.com/nicko101/Enterprise-Architecture-Portfolio/main/resources/slides/infranew.png)
*Figure 1: Detailed infrastructure map showing the integration of local virtualization hosts, physical switching, and cloud-native gateways.*

---

## Core Networking Components

### Cloud Networking
* **Virtual Network (VNet) Fabric**: Implementation of segmented address spaces and subnetting within the Azure tenant.
* **VPN Gateway**: Configuration of the Azure Virtual Network Gateway for route-based IPsec termination.
* **Private DNS Resolver**: Integration for seamless cross-premises name resolution between the local lab and cloud services.

### On-Premises Switching and VLANs
* **Traffic Segmentation**: Logical isolation of Management, Corporate, Guest, and IoT traffic using 802.1Q tagging.
* **Trunking and Aggregation**: Layer 2 configurations connecting the Proxmox virtual cluster to the network fabric.
* **Spanning Tree Protocol (STP)**: Configuration for loop prevention and redundant pathing across the physical switching layer.

### Wireless and Identity-Based Access
* **Identity Authority**: Integration with Aruba ClearPass for Corporate (EAP-TLS) and Guest SSID management.
* **RADIUS Orchestration**: Technical integration between the wireless controller and the ClearPass policy engine for real-time authentication.

### Edge Security and Routing
* **Hybrid Tunneling**: Palo Alto NGFW serves as the primary gateway, terminating the hybrid tunnel using IKEv2 and AES-256 encryption.
* **Routing Strategy**: Implementation of predictable traffic flow and failover capabilities between the edge and the cloud gateway.

### Virtualization Networking (Proxmox SDN)
* **Virtual Bridges**: Software-defined networking configurations designed to support high-density virtual machine workloads.
* **Protocol Forwarding**: Specialized kernel bridge masks allowing 802.1X packets to pass transparently to the NAC engine.

---

## Operational Validation

[![Operational Validation](https://raw.githubusercontent.com/nicko101/Enterprise-Architecture-Portfolio/main/resources/slides/groupmask.png)](https://raw.githubusercontent.com/nicko101/Enterprise-Architecture-Portfolio/main/resources/slides/groupmask.png)
*Figure 2: Validation of specialized bridge settings required for Zero Trust network admission control.*

---
[Return to Root README](../../README.md)
