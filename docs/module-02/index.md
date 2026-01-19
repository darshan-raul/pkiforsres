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
