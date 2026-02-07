Packet Life: Forensic Timeline & Validation
🏛️ Executive Summary
This repository provides a technical synthesis of a hybrid network environment utilizing Azure Resource Manager (ARM) templates and Palo Alto Virtual Machine (PA-VM) security appliances. It documents the 'Packet Life Walk'—the binary proof of identity-based access, deterministic routing, and cloud-ingress integrity.

🗺️ The Validated Packet Journey
1. Identity Admission & Edge Security
Evidence: Digital Identity Card

Logic: Implementation of TEAP (EAP-TLS, EAP-MSCHAPv2) via Aruba ClearPass.

Outcome: Verified session for LAB02\nick on device Win10 (bc:24:11:fb:cb:c4), authorizing network access on Feb 07, 2026.

2. L3 Mapping & Deterministic Provisioning
Evidence: DHCP Assignment

Logic: The PA-VM DHCP server binds the authenticated identity to a specific IP.

Outcome: Host Win10 committed to IPv4 address 10.0.11.17 from the 10.0.11.0/24 pool.

3. Tunnel Steering & Policy Enforcement
Evidence: PBF Routing Logic

Logic: Policy-Based Forwarding (PBF) explicitly routes traffic from the local LAN through 'tunnel.200' for egress.

Outcome: Traffic is correctly steered via PBF rule 'Simulate-Default-Route' to tunnel.200.

4. Cloud Ingress & Forensic Analysis
Evidence: Packet Capture Analysis

Logic: Traffic traverses an Azure Standard Load Balancer (nf-elb) to the PA-VM backend pool.

Diagnostic Insight: PCAP analysis confirms bidirectional DNS flow and active TCP management (RST/ACK), proving end-to-end path validity.

🕵️ Technical Diagnostics
Azure LB: Regional 'Standard' SKU (nf-elb) deployed in West Europe.

Health Probes: Port 443 is monitored every 5 seconds to ensure backend availability.

Traffic Rules: Supports multi-protocol traffic including TCP (80, 443, 22) and UDP (500, 4500) with SourceIP load distribution.

✅ Final Status: VALIDATED
End-to-end architectural integrity is confirmed through the cross-correlation of identity logs, PBF enforcement, and packet-level captures.
