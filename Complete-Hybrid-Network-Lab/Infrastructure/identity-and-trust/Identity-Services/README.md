# 🆔 Identity Services Hub

## 📖 Overview
This directory contains the foundational services that manage identity, authentication, and authorization across the hybrid fabric. In this architecture, identity is the primary perimeter.

## 📂 Sub-Directory Map

### 🛡️ [RBAC & Governance](./RBAC)
Focuses on Role-Based Access Control, Entra ID App Registrations, and machine-to-machine (M2M) identity governance.

### 📜 [Certificate Services (PKI)](../Certificate-Services)
*Note: Linked to the Infrastructure root.* The Root of Trust for all certificate-based identity deployment via SCEP/NDES.

## 🖼️ Identity Architecture
![Identity Handshake](../../../resources/images/Anatomy%20of%20a%20Modern%20Secure%20Network.png)
*Figure 1: The Secure Connection Flow - Integrating Intune, ClearPass, and Palo Alto via a unified identity plane.*
