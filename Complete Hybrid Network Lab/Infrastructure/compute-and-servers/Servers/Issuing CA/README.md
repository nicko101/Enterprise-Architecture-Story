
# 📜 Server Role: Online Issuing CA (lab02-INTERCA-CA)

## 📖 Overview
The operational certificate authority responsible for automated certificate lifecycle management. This server bridges the Root of Trust to live endpoints, facilitating **802.1X authentication** and secure communication.

## 🛠️ Configuration & Role
* **Hierarchy Position**: Level 1 — Subordinate (Issuing) CA.
* **Identity Protocol**: Supports certificate issuance via RPC and SCEP for domain-joined and mobile endpoints.
* **Availability**: Configured with accessible AIA and CDP locations to ensure certificate validation succeeds across the hybrid fabric.

## 🖼️ Operational Evidence
![PKI Health Status](../../Infrastructure/Certificate-Services/PKI/pki-health.png)
*Figure 1: pkiview validation showing "OK" status for all certificate distribution points.*
