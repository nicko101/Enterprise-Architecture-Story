
# 🏛️ Server Role: Offline Root Certificate Authority

## 📖 Overview
The ultimate anchor of trust for the Enterprise PKI hierarchy (`lab02-DC01-CA`). This server is architected as an offline node to ensure the integrity of the private key, following enterprise-grade security best practices.

## 🛠️ Configuration & Role
* **Hierarchy Position**: Level 0 — Root CA.
* **Function**: Issued the digital certificate for the Issuing CA and defines the validity period for the entire trust chain.
* **Security Posture**: Maintained in an offline state; only activated for CRL updates or subordinate certificate renewal.

## 🖼️ Operational Evidence
![PKI Health Status](../../Infrastructure/Certificate-Services/PKI/pki-health.png)
*Figure 1: Validation of the Root-CA certificate status within the Enterprise PKI hierarchy.*
