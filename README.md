# 📂 Packet Life: Forensic Timeline & Validation

### 🏛️ Executive Summary
This repository serves as the **Forensic Flight Recorder** for the Hybrid Cloud Architecture. It documents the 'Packet Life Walk'—the binary proof of identity-based access, deterministic routing, and cloud-ingress integrity.

---

### 🗺️ The Validated Packet Journey

#### **1. Identity Admission & Edge Security**
* **Statement of Proof**: [Digital Identity Card (slide-cppm.png)](Packet-Life-Forensics/slide-cppm.png)
* **Validation**: Implementation of **TEAP (EAP-TLS, EAP-MSCHAPv2)** via Aruba ClearPass.
* **Outcome**: Verified session for `LAB02\nick` on device `Win10` (`bc:24:11:fb:cb:c4`), authorizing network access on Feb 07, 2026.

#### **2. L3 Mapping & Deterministic Provisioning**
* **Statement of Proof**: [DHCP Assignment (slide-dhcp.png)](Packet-Life-Forensics/slide-dhcp.png)
* **Validation**: The PA-VM DHCP server binds the authenticated identity to a specific IP.
* **Outcome**: Host `Win10` committed to IPv4 address **10.0.11.17** from the 10.0.11.0/24 pool.

#### **3. Tunnel Steering & Policy Enforcement**
* **Statement of Proof**: [PBF Routing Logic (slide-pbf.png)](Packet-Life-Forensics/slide-pbf.png)
* **Validation**: **Policy-Based Forwarding (PBF)** explicitly routes traffic through 'tunnel.200' for egress.
* **Outcome**: Traffic is correctly steered via PBF rule 'Simulate-Default-Route' to **tunnel.200**.

#### **4. Cloud Ingress & Forensic Analysis**
* **Statement of Proof**: [Packet Capture Analysis (slide-pcap.png)](Packet-Life-Forensics/slide-pcap.png)
* **Artifact**: [Raw Capture (save.pcap)](Packet-Life-Forensics/save.pcap)
* **Diagnostic Insight**: PCAP analysis confirms bidirectional DNS flow and active TCP management (RST/ACK), proving end-to-end path validity.

---

### 🕵️ Technical Diagnostics
* **Azure LB**: Regional 'Standard' SKU (nf-elb) deployed in West Europe.
* **Health Probes**: Port 443 is monitored every 5 seconds to ensure backend availability.
* **Traffic Rules**: Supports multi-protocol traffic including TCP (80, 443, 22) and UDP (500, 4500) with SourceIP load distribution.

### ✅ Final Status: [ARCHITECTURAL VALIDATION COMPLETE](Packet-Life-Forensics/slide-summart.png)
End-to-end architectural integrity is confirmed through the cross-correlation of identity logs, PBF enforcement, and packet-level captures.
