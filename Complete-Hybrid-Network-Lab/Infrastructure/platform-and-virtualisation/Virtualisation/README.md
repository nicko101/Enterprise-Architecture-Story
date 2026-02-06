# Virtualisation: The Infrastructure Foundation

## Overview
This directory documents the virtualization and network simulation layers that host the hybrid lab environment. By utilizing a combination of Type-1 hypervisors and advanced network emulation, the lab achieves production-grade complexity within a controlled, scalable local footprint.

---

## Core Virtualisation Pillars

### Proxmox VE (Type-1 Hypervisor)
The foundational compute layer is responsible for hosting all high-availability server workloads and virtual security appliances.
* Workload Isolation: Individual LXC containers and VMs are used to isolate critical services like the Offline Root CA and NDES server.
* Resource Management: Dynamic allocation of CPU, RAM, and Storage to ensure optimal performance for the Palo Alto and ClearPass nodes.

### GNS3 (Network Emulation)
The primary engine for network topology simulation and deep-packet analysis.
* Topology Complexity: Simulates the multi-site switching fabric, including Aruba and Cisco virtual images.
* Hybrid Bridge: Connects the virtual GNS3 network directly to the Proxmox VM network, allowing seamless communication between simulated switches and production-ready servers.

---

## Operational Evidence

[![Virtualization Inventory and Component Stack](../../../resources/slides/fullstack.png)](../../../resources/slides/fullstack.png)
*Figure 1: Inventory of virtualization management components and the integrated VNF stack within the lab infrastructure.*

---

## Performance and Standards
* Hyper-Converged Infrastructure: Compute and storage are unified within the Proxmox cluster to reduce latency for high-demand security workloads.
* Network Fidelity: GNS3 utilizes actual vendor OS images (ArubaOS, PAN-OS) to ensure that security policies behave exactly as they would in a physical environment.
* Snapshot and Recovery: Automated VM snapshots are performed before major architectural changes to ensure a development environment with minimal operational risk.

---
[Return to Infrastructure](../) | [Return to Lab Root](../../README.md)
