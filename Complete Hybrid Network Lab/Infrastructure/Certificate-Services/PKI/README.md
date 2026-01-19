
🏗️ PKI Trust Chain: Root & Issuing CA Implementation
📖 Overview
The foundation of the lab's Zero Trust architecture is a two-tier Enterprise Public Key Infrastructure (PKI). This hierarchy ensures that identity is cryptographically verified across all hybrid components, from on-premises switches to cloud-managed endpoints.

🏛️ The Two-Tier Hierarchy
To align with production security standards, the PKI is split into two distinct functional layers:

1. Offline Root CA (Root CA)
Role: The ultimate "Trust Anchor" for the entire modernization suite.

Implementation: A standalone, non-domain-joined Windows Server 2022 instance.

Security Policy: This server remains offline and powered down after issuing the initial certificate to the Subordinate CA to prevent root key compromise.

Primary Task: Issuing the Certificate Revocation List (CRL) and the Subordinate CA certificate.

2. Online Issuing CA (Issuing CA)
Role: The operational engine that handles daily certificate requests.

Implementation: An Enterprise Subordinate CA joined to the Active Directory domain.

Security Policy: Managed via Certificate Templates with granular permissions for automated enrollment.

Primary Task: Issuing identity certificates to users, devices, and NDES servers for SCEP workflows.

⚙️ Technical Integration Flow
The trust chain is established through the following verified steps:

Root Generation: A 4096-bit RSA key pair is generated on the Root CA.

Subordinate Request: The Issuing CA generates a Certificate Signing Request (CSR).

Cross-Signing: The CSR is manually moved to the Root CA via secure media, signed, and the resulting certificate is imported back to the Issuing CA.

AIA/CDP Hosting: The Authority Information Access (AIA) and CRL Distribution Point (CDP) are hosted on a highly available web server (or the Issuing CA) for client verification.
