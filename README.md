# 📂 Packet Life: Forensic Data Plane Validation

### 🏛️ Project Purpose
This repository serves as the **Forensic Flight Recorder** for the Hybrid Cloud Architecture. While the Enterprise Portfolio defines the design intent, this repository contains the **Packet-Level Proof** that the data plane behaves according to the defined security and steering policies.

### 🗺️ The "Packet Life" Framework
I validate the lifecycle of a packet through four distinct architectural "Checkpoints" to ensure end-to-end integrity:

#### **1. Identity-Based Admission (The Edge)**
* **Logic**: No traffic is permitted until **TEAP (Chained Authentication)** is successful.
* **Validation**: ClearPass logs confirm both user `LAB02\nick` and machine `Win10` are authenticated.

#### **2. Deterministic Provisioning (The L3 Boundary)**
* **Logic**: The Palo Alto PA-VM assigns a specific identity-linked IP via DHCP.
* **Validation**: Forensic matching of MAC `bc:24:11:fb:cb:c4` to IP `10.0.11.17`.

#### **3. Policy-Based Forwarding (The Steering)**
* **Logic**: Standard routing is overridden to steer traffic into specific encrypted tunnels.
    * **Internet Traffic**: Steered to `tunnel.200`.
    * **Internal Azure VNet (172.17.x.x)**: Steered to `tunnel.99`.
* **Validation**: Traffic logs confirm `8.8.8.8` egresses via `tunnel.200` as intended.

#### **4. Cloud Ingress & Forensic Analysis (The Arrival)**
* **Logic**: Traffic arrives at the **Azure NVA** after traversing the **Standard Load Balancer (nf-elb)**.
* **Validation**: Wireshark analysis (`rx(7).pcap`) confirms decapsulated packet arrival and DNS resolution continuity.

---

### 🏛️ Key Forensic Artifacts
* **`rx(7).pcap`**: Proof of cloud-side arrival for internet-bound traffic.
* **`Auth-Success.png`**: ClearPass evidence of the TEAP handshake.
* **`PBF-Logic.log`**: Proof of steering via Policy-Based Forwarding.



### 🚀 Senior Architect Insight
The absence of certain traffic in specific captures (e.g., missing `172.17.x.x` traffic in an internet-focused PCAP) is used to verify **Path Isolation**. This confirms that PBF rules are successfully separating sensitive internal traffic from general internet traffic at the encryption boundary, ensuring no "leaks" occur between the cloud spokes.
