# Module 10: Zero Trust & Modern PKI

**Goal:** The Future.

## 31. Zero Trust PKI

"Assume Breach."
Assume the network is hostile.
Therefore, EVERY connection must be mTLS.

### SPIFFE / SPIRE
*   **SPIFFE**: A standard for identity. `spiffe://my-trust-domain/ns/default/sa/my-app`
*   **SPIRE**: The software that issues these certs automatically. Workload Identity.

## 32. Advanced TLS

*   **ECH (Encrypted Client Hello)**: Hides the SNI so the ISP doesn't know which website you are visiting on a shared IP.
*   **QUIC (HTTP/3)**: TLS 1.3 built directly into UDP. Faster, better for mobile roaming.

---

## Labs

- [Lab 10.1: Enabling HTTP/3 (QUIC)](../labs/module-10/lab-10-1.md)
