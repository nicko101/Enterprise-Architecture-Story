# 📂 Packet Life: Forensic Timeline & Validation

### 🏛️ Executive Summary
This repository serves as the **Forensic Flight Recorder** for the Hybrid Cloud Architecture. It documents the 'Packet Life Walk'—the binary proof of identity-based access, deterministic routing, and cloud-ingress integrity.

---

### 🗺️ The Validated Packet Journey

#### **1. Identity Admission & Edge Security**
* **Statement of Proof**: [Digital Identity Card](Packet-Life-Forensics/slide-cppm.png)
* **Validation**: Implementation of **TEAP (EAP-TLS, EAP-MSCHAPv2)** via Aruba ClearPass.
* **Outcome**: Verified session for `LAB02\nick` on device `Win10` (`bc:24:11:fb:cb:c4`), authorizing network access on Feb 07, 2026.

#### **2. L3 Mapping & Deterministic Provisioning**
* **Statement of Proof**: [DHCP Assignment](Packet-Life-Forensics/slide-dhcp.png)
* **Validation**: The PA-VM DHCP server binds the authenticated identity to a specific IP.
* **Outcome**: Host `Win10` committed to IPv4 address **10.0.11.17** from the 10.0.11.0/24 pool.

#### **3. Tunnel Steering & Policy Enforcement**
* **Statement of Proof**: [PBF Routing Logic](Packet-Life-Forensics/slide-pbf.png)
* **Validation**: **Policy-Based Forwarding (PBF)** explicitly routes traffic through 'tunnel.200' for egress.
* **Outcome**: Traffic is correctly steered via PBF rule 'Simulate-Default-Route' to **tunnel.200**.

#### **4. Cloud Ingress & Forensic Analysis**
* **Statement of Proof**: [Packet Capture Analysis](Packet-Life-Forensics/slide-pcap.png)
* **Artifact**: [Raw Data Plane Capture](Packet-Life-Forensics/save.pcap)
* **Diagnostic Insight**: PCAP analysis confirms bidirectional DNS flow and active TCP management (RST/ACK), proving end-to-end path validity.

---

### 🕵️ Technical Diagnostics
* **Azure LB**: Regional 'Standard' SKU (nf-elb) deployed in West Europe.
* **Health Probes**: Port 443
