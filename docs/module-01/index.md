# Module 1: Foundations of PKI

**Goal:** Build a correct mental model before touching protocols.

Before we dive into certificates and TLS, we need to understand the fundamental building blocks. PKI isn't magic; it's a chain of trust built on specific mathematical properties.

## 1. Setting the Premise

### What is PKI really solving?

At its core, Public Key Infrastructure (PKI) solves the problem of **trust at scale**.

*   **Identity**: "Are you who you say you are?" (Authentication)
*   **Confidentiality**: "Can anyone else read this?" (Encryption)
*   **Integrity**: "Has this been tampered with?" (Signing)

In a web context, how does your browser know that `google.com` is actually Google servers and not a hacker in a coffee shop? PKI provides the infrastructure to answer that.

### Public Key vs Private Key

*   **Private Key**: The secret. **NEVER** share this. It stays on the server/device.
*   **Public Key**: The lock. Share this with everyone.

> **Analogy:**
> *   **Public Key** = Your mailbox slot. Anyone can drop a message in (encrypt).
> *   **Private Key** = The key to open the mailbox. Only you can read the messages (decrypt).

### Trust Models

*   **Web PKI**: Relies on a set of pre-trusted "Root CAs" shipped with your OS/Browser (DigiCert, Let's Encrypt, etc.). The entire internet relies on these few dozen entities.
*   **Enterprise PKI**: You control the Root CA. You install the trust anchor on your company laptops. Used for internal traffic, VPNs, mTLS.

## 2. Crypto & Algorithm Landscape

### RSA vs ECDSA vs Ed25519

*   **RSA**: The grandfather. Reliable, compatible everywhere. Keys are huge (2048 or 4096 bits) and slower to generate.
*   **ECDSA (Elliptic Curve)**: Modern standard. Smaller keys (256 bits = 3072 bit RSA strength), faster, less CPU intensive.
*   **Ed25519**: A specific curve (EdDSA). Very fast, resistant to side-channel attacks. The darling of SSH and modern crypto (WireGuard), but slower adoption in Web PKI.

### Diffie-Hellman & ECDHE

How do two people who have never met agree on a secret password over a public channel?
**Diffie-Hellman (DH)** allows this key exchange.
*   **ECDHE (Elliptic Curve Diffie-Hellman Ephemeral)**: The "Ephemeral" part is key. It generates a temporary key for *just this session*. If the server's long-term private key is stolen later, the attacker *cannot* decrypt past traffic. This is **Forward Secrecy**.

### Symmetric vs Asymmetric Crypto

*   **Asymmetric (Public/Private)**: Slow. Used *only* for the handshake (proving identity and exchanging a symmetric key).
*   **Symmetric (AES, ChaCha20)**: Fast. Used for the actual data transfer.

> **TLS Workflow**: Use Asymmetric crypto to agree on a Symmetric key, then switch to Symmetric for speed.

### Hashing (SHA256, SHA3)

One-way fingerprints. You can't turn the hash back into data.
*   Used for: Verifying file integrity, signing certificates.
*   **SHA-1**: Broken. Do not use.
*   **SHA-256**: The current standard.

## 3. Encryption vs Signing

This is often confused. It's the same math, but reversed roles.

| Operation | Action | Key Used | Goal |
| :--- | :--- | :--- | :--- |
| **Encryption** | Encrypt | **Receiver's Public Key** | Confidentiality (Only they can read) |
| **Decryption** | Decrypt | **Receiver's Private Key** | |
| **Signing** | Sign | **Sender's Private Key** | Authenticity (Proof I wrote it) |
| **Verification** | Verify | **Sender's Public Key** | Integrity (It hasn't changed) |

### Real-world examples
*   **Encryption**: Sending a credit card number to Amazon.
*   **Signing**: A JWT token, a git commit signature, a TLS Certificate (CA signs the cert).

---

## Labs

- [Lab 1.1: Generate Keys & Compare](../labs/module-01/lab-1-1.md)
- [Lab 1.2: Digital Signatures](../labs/module-01/lab-1-2.md)
