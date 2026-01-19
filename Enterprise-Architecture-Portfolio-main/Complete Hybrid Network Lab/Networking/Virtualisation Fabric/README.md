# Virtualisation Fabric: Compute and Logical Bridging

## Overview
This directory documents the underlying hypervisor and simulation layers that host the infrastructure. The fabric is powered by Proxmox VE and GNS3, providing a high-performance environment for virtualized network functions (VNFs) and server workloads.

[![Virtualisation and Bridge Architecture](../../../resources/slides/anatomy.png)](../../../resources/slides/anatomy.png)
*Figure 1: Validation of the bridge configuration and resource allocation required to support the Zero Trust NAC workflow.*

---

## Architecture and Integration
The virtualisation tier is a critical participant in the Zero Trust security model, providing the necessary protocol transparency for complex 802.1X authentication flows.

### Proxmox SDN and Bridge Hardening
A standard virtual bridge often drops specialized Layer 2 frames. This fabric utilizes a hardened Linux Bridge (vmbr25) to facilitate security handshakes.
* EAPOL Transparency: Configured with group_fwd_mask to allow 802.1X authentication frames to pass from GNS3 switches to the ClearPass Policy Manager.
* VLAN Trunks: Provides 802.1Q tagging support, allowing the Palo Alto VM to segment traffic across the entire virtual environment.

### GNS3 Network Simulation
The GNS3 layer hosts the virtualized switching fabric, including Aruba AOS-CX and Cisco IOSv appliances.
* VNF Hosting: Runs the specialized images required for authentic network behavior, ensuring the Aruba AOS-CX-01 configuration behaves exactly like physical hardware.
* TAP Interconnects: Uses tap interfaces to bridge the simulated GNS3 network directly into the Proxmox kernel for low-latency communication.

---

## Infrastructure Standards
* Resource Overcommitment: Managed via KVM/QEMU to optimize CPU and RAM usage for the multi-vendor environment.
* Persistence: Critical nodes, such as Domain Controllers and ClearPass, utilize automated snapshotting to ensure rapid recovery during testing cycles.
* Hardware Performance: Implementation of virtio drivers to provide high-throughput performance to the Palo Alto perimeter and core switching components.

---
[Return to Networking](../) | [Return to Lab Root](../../README.md)
