# Module 2: TLS & HTTPS Deep Dive

**Goal:** Master what SREs actually operate every day.

This module is the bread and butter of SRE work. 90% of PKI tickets will be related to what's in this section.

## 4. HTTPS / TLS Complete Workflow

### The TLS Handshake (Simplified 1.3)

1.  **Client Hello**: "I speak TLS 1.3, here are my supported ciphers, and here is my key share (ECDHE)."
2.  **Server Hello**: "Let's use TLS 1.3, ChaCha20, and here is my key share."
    *   **Certificate**: "Here is my ID card (Certificate)."
    *   **Finished**: "I'm ready to switch to encryption."
3.  **Client Finished**: "Verified your ID. Switching to encryption."

> **Note:** In TLS 1.3, this happens in 1 Round Trip (1-RTT). In TLS 1.2, it took 2-RTT.

## 5. Certificate Types

### Trust levels
*   **Self-Signed**: You signed it yourself. Browsers hate this. Good for internal testing only if you manually trust it.
*   **Private CA**: Your company signed it. Trusted by company laptops, not by the public.
*   **Public CA**: Let's Encrypt, DigiCert, etc. Trusted by everyone.

### Verification (Class)
*   **DV (Domain Validated)**: "I control the DNS." (Let's Encrypt style).
*   **OV (Organization Validated)**: "I am who I say I am" (Paperwork involved).
*   **EV (Extended Validation)**: "I am a super legit bank." (Green bar - mostly dead now).

## 6. Certificate Anatomy

*   **Subject**: Who is this for? (`CN=example.com`)
*   **Issuer**: Who signed this? (`CN=Let's Encrypt R3`)
*   **SAN (Subject Alternative Name)**: The *real* list of valid domains. **Browsers ignore CN and look at SAN.**
*   **Validity**: `Not Before` and `Not After`.
*   **Extensions**:
    *   `Key Usage`: Digital Signature, Key Encipherment.
    *   `Extended Key Usage` (EKU): Server Auth, Client Auth.
    *   `Basic Constraints`: CA=FALSE (This cert cannot sign other certs).

## 7. TLS Versions

*   **SSL 1, 2, 3**: Dead. Insecure.
*   **TLS 1.0, 1.1**: Deprecated. Disable them.
*   **TLS 1.2**: Minimum standard. Safe if configured correctly.
*   **TLS 1.3**: The gold standard. Faster (0-RTT/1-RTT), safer (no weak ciphers allowed).

## 8. Operational TLS Topics

### Rotation logic
*   **Manual**: Ticket to Ops -> Generate CSR -> Send to Vendor -> Wait -> Deploy. (Bad)
*   **Automated (ACME)**: `cert-bot` talks to Let's Encrypt -> Prove ownership -> Get Cert -> Reload Nginx. (Good)

### Certificate Pinning (HPKP)
*   **Don't do it.** It caused more outages than it prevented.
*   Use **Certificate Transparency (CT)** logs instead to monitor for authorized issuance.

## 9. Revocation

How do we kill a cert before it expires?

*   **CRL (Certificate Revocation List)**: A giant list of bad serial numbers. Slow, inefficient.
*   **OCSP (Online Certificate Status Protocol)**: Ask the CA "Is serial #123 good?" in real-time. Privacy leak.
*   **OCSP Stapling**: The Server asks the CA "Give me a signed proof this cert is good", and sends that proof to the Client. Best of both worlds.

---

## Labs

- [Lab 2.1: Build your own CA](../labs/module-02/lab-2-1.md)
- [Lab 2.2: HTTPS Server](../labs/module-02/lab-2-2.md)
- [Lab 2.3: Break TLS](../labs/module-02/lab-2-3.md)
- [Lab 2.4: Cert Rotation](../labs/module-02/lab-2-4.md)
- [Lab 2.5: OCSP & Revocation](../labs/module-02/lab-2-5.md)
- [Lab 2.6: TLS Versions & Performance](../labs/module-02/lab-2-6.md)

## Jargons to cover:

Here’s a breakdown of the **complex jargons in PKI (Public Key Infrastructure)**, explained in the context of your fintech and cloud infrastructure experience. I’ll group them by function and include why they matter for your work:

---

### **1. Core PKI Components**
| Term                     | What It Means                                                                                     | Why It Matters for You                                                                                     |
|--------------------------|---------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------|
| **Certificate Authority (CA)** | Trusted entity that issues, signs, and revokes digital certificates.                              | Your org might run a private CA (e.g., AWS Private CA) to issue certs for internal services.               |
| **Registration Authority (RA)** | Optional intermediary that verifies identities before the CA issues a certificate.              | Useful for compliance (e.g., KYC in fintech) before issuing user certs.                                    |
| **Certificate Revocation List (CRL)** | A list of revoked certificates published by the CA.                                             | Critical for invalidating compromised certs (e.g., if a dev leaves your team).                              |
| **Online Certificate Status Protocol (OCSP)** | Real-time protocol to check if a certificate is revoked.                                         | Faster than CRLs, but requires OCSP responder infrastructure (e.g., AWS Private CA supports OCSP).       |
| **OCSP Stapling**        | Server caches OCSP responses to reduce client-side latency.                                      | Improves performance for TLS connections in your APIs or web apps.                                        |

---

### **2. Certificate Types and Formats**
| Term                     | What It Means                                                                                     | Why It Matters for You                                                                                     |
|--------------------------|---------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------|
| **X.509 Certificate**    | Standard format for PKI certificates (e.g., `.crt`, `.pem`, `.der`).                             | Used for TLS, code signing, and SSH certs in your infrastructure.                                          |
| **CSR (Certificate Signing Request)** | Request file (e.g., `.csr`) sent to a CA to apply for a certificate.                              | You generate these when requesting certs for your load balancers or internal services.                     |
| **Subject Alternative Name (SAN)** | Extends certs to cover multiple domains/IPs (e.g., `api.yourfintech.com`, `10.0.0.1`).          | Essential for multi-tenant cloud apps or internal services with dynamic IPs.                                |
| **Wildcard Certificate** | Covers all subdomains (e.g., `*.yourfintech.com`).                                                | Simplifies cert management for microservices, but avoid overuse (security risk).                           |
| **End-Entity Certificate** | Cert issued to a user/server (not a CA).                                                          | Your EC2 instances, APIs, or user devices use these.                                                        |
| **Intermediate Certificate** | Cert issued by a root CA to another CA (forms a chain of trust).                                  | AWS Private CA uses these to delegate trust for internal certs.                                            |
| **Root Certificate**     | Top-level cert in the trust chain (self-signed).                                                  | Pre-installed in OS/browser trust stores; your private CA’s root must be trusted by all clients.            |
| **Self-Signed Certificate** | Cert signed by its own private key (no CA).                                                     | Useful for testing, but **never** for production (e.g., your dev environments).                             |

---

### **3. Cryptographic Algorithms**
| Term                     | What It Means                                                                                     | Why It Matters for You                                                                                     |
|--------------------------|---------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------|
| **RSA**                  | Asymmetric algorithm for key generation and signing.                                             | Legacy but still used; avoid for new deployments (prefer ECC).                                               |
| **ECC (Elliptic Curve Cryptography)** | Asymmetric crypto using elliptic curves (e.g., `secp256r1`, `secp384r1`).                        | Faster and stronger than RSA at smaller key sizes (e.g., ECDSA for TLS in AWS).                             |
| **EdDSA (Ed25519/Ed448)** | Modern ECC-based signature scheme.                                                                | Used in SSH certs and TLS; more efficient than RSA/ECDSA.                                                   |
| **AES**                  | Symmetric encryption for data at rest/transit.                                                   | Used in TLS cipher suites (e.g., `AES-256-GCM`).                                                          |
| **SHA-256/SHA-384**      | Hash functions for certificate fingerprints and signatures.                                      | Ensure your certs use SHA-2 or SHA-3 (avoid SHA-1).                                                        |
| **DSA**                  | Older signing algorithm (deprecated).                                                            | Avoid; vulnerable to attacks.                                                                              |

---

### **4. PKI Protocols and Standards**
| Term                     | What It Means                                                                                     | Why It Matters for You                                                                                     |
|--------------------------|---------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------|
| **TLS/SSL**              | Protocols for encrypted communication (uses X.509 certs).                                       | Your APIs, load balancers, and internal services rely on this.                                             |
| **S/MIME**               | Standard for encrypting/signing emails.                                                          | Relevant if your fintech handles sensitive email (e.g., customer statements).                               |
| **CMP (Certificate Management Protocol)** | Protocol for CA enrollment and management.                                                     | Used in enterprise PKI (e.g., AWS Private CA supports CMP).                                                |
| **EST (Enrollment over Secure Transport)** | Protocol for certificate enrollment (simpler than CMP).                                         | Useful for IoT devices or automated cert issuance in your cloud.                                           |
| **PKCS#12 (.p12/.pfx)**  | Binary format for storing certs + private keys (password-protected).                             | Used to import certs into AWS ACM or your app servers.                                                     |
| **PKCS#7 (.p7b)**        | Format for cert chains (no private key).                                                         | Used to distribute CA bundles (e.g., for client trust stores).                                             |

---

### **5. Advanced PKI Concepts**
| Term                     | What It Means                                                                                     | Why It Matters for You                                                                                     |
|--------------------------|---------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------|
| **Certificate Transparency (CT)** | Public log of issued certs to detect misuse (e.g., fraudulent certs for your domain).           | Critical for domain validation; AWS Private CA supports CT logs.                                           |
| **Short-Lived Certificates** | Certs with very short validity (e.g., hours/days).                                               | Reduces risk (e.g., for service-to-service auth in AWS).                                                   |
| **ACME (Automatic Certificate Management Environment)** | Protocol for automated cert issuance (e.g., Let’s Encrypt).                                    | Used by AWS ACM for public TLS certs; can integrate with your private CA for internal certs.               |
| **HSM (Hardware Security Module)** | Physical device to securely generate/store private keys.                                        | AWS CloudHSM or KMS can protect your CA’s private keys.                                                   |
| **Key Escrow**           | Storing private keys with a third party for recovery.                                             | Rarely used; avoid unless required by compliance.                                                          |
| **Non-Repudiation**      | Ensures a signed action (e.g., transaction) cannot be denied by the signer.                     | Critical for fintech audits (e.g., code signing or API requests).                                          |

---

### **6. PKI in Your AWS/Fintech Context**
- **AWS Private CA**: Lets you issue internal certs for EC2, containers, or APIs (integrates with IAM for access control).
- **ACM (AWS Certificate Manager)**: Manages public TLS certs (e.g., for ALB or CloudFront).
- **IAM + PKI**: Use IAM policies to restrict who can request certs from your private CA.
- **ABAC + Certificates**: Embed attributes (e.g., `department=finance`) in certs to enforce granular access.

---

### **Example: PKI Workflow for Your Infrastructure**
1. **Issue a Cert**:
   - Your app requests a cert via **ACME** or **CMP** from AWS Private CA.
   - The CA validates the request (e.g., checks IAM permissions).
2. **Deploy the Cert**:
   - The cert (with SANs for your internal services) is deployed to an **ALB** or **ECS task**.
3. **Revocation**:
   - If a cert is compromised, it’s added to the **CRL** or checked via **OCSP**.

---

### **Pitfalls to Avoid**
- **Long-Lived Certs**: Use short validity (e.g., 90 days) and automate renewal.
- **Weak Algorithms**: Avoid RSA < 2048-bit or SHA-1.
- **Manual Processes**: Automate enrollment/revocation (e.g., with Lambda + AWS Private CA).

