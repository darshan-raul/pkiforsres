# Module 8: PKI Operations

**Goal:** How to run a CA without getting fired.

## 26. PKI Lifecycle

1.  **Generation**: Creating the key. Ideally inside an HSM.
2.  **CSR**: Requesting a signature.
3.  **Issuance**: The CA checks the CSR and signs it.
4.  **Distribution**: Getting the cert to the server (`scp`, `vault`, `k8s secret`).
5.  **Renewal**: 30 days before expiry, do it all again.
6.  **Revocation**: Emergency kill switch.

## 27. Enterprise PKI Design

**The Golden Rule**: The Root CA lives Offline.
*   **Root CA**: Generated on a laptop with no Wifi card. Stored in a safe. Used *once* every 5 years to sign an Intermediate.
*   **Intermediate CA**: Online (or less secure). Signs the leaf certs. If compromised, you revoke it using the Root (from the safe), and issue a new Intermediate.

## 28. HSMs (Hardware Security Modules)

A specific computer that never lets the private key leave.
*   **AWS KMS / Google Cloud HSM**: Managed versions.
*   **YubiKey**: A tiny personal HSM.

---

## Labs

- [Lab 8.1: Hashicorp Vault PKI Engine](../labs/module-08/lab-8-1.md)
- [Lab 8.2: HSM Simulation (SoftHSM)](../labs/module-08/lab-8-2.md)
