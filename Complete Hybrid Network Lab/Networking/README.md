# Network Infrastructure: Hybrid Connectivity Fabric

## Overview
This directory documents the core networking architecture for the environment. The design utilizes a hub-and-spoke model to bridge on-premises physical and virtualized infrastructure with Microsoft Azure, prioritizing secure tunneling, predictable path selection, and high-performance throughput.

[![Network Infrastructure Overview](https://raw.githubusercontent.com/nicko101/Enterprise-Architecture-Portfolio/main/resources/slides/infra.png)](https://raw.githubusercontent.com/nicko101/Enterprise-Architecture-Portfolio/main/resources/slides/infra.png)
*Figure 1: Detailed infrastructure map showing the integration of local virtualization hosts, physical switching, and cloud-native gateways.*

---

## Core Networking Components

### Cloud Connectivity
* **Azure VPN Gateway**: Configuration of a route-based Virtual Network Gateway for secure IPsec termination.
* **Hybrid Tunneling**: Implementation of IKEv2 tunnels with AES-256 encryption to ensure data integrity across public transit.
* **VNet Integration**: Architecture of the cloud landing zone to support hybrid workload communication.

### On-Premises Fabric
* **Traffic Segmentation**: Logical isolation of Management, Corporate, Guest, and IoT traffic using 802.1Q VLAN tagging.
* **Edge Routing**: Palo Alto NGFW serves as the primary gateway, managing route advertisements and security inspection for cross-premises traffic.
* **Layer 2 Infrastructure**: Physical and virtual switching configurations designed to support high-availability compute nodes.

### Virtualization Networking
* **Software-Defined Bridges**: Proxmox-based bridge configurations that allow virtual machines to communicate directly with the physical network fabric.
* **Security Transit**: Optimized paths for RADIUS and management traffic between simulated GNS3 nodes and physical enforcement points.

---

## Technical Validation
Networking success is verified through:
* **Route Verification**: Confirmation of BGP/Static route propagation across the hybrid link.
* **Tunnel Stability**: Monitoring of IPsec security associations and uptime metrics.
* **Traffic Inspection**: Verification of application-layer visibility on the edge firewall.

---
[Return to Root README](../../README.md)
