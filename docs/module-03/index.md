# Module 3: SSH PKI

**Goal:** Understand how SSH uses PKI, which is often different from the Web PKI (TLS).

## 11. SSH Complete Workflow

SSH (Secure Shell) uses similar math to TLS but has a different handshake and trust model.

1.  **Key Exchange (KEX)**: Diffie-Hellman to agree on a session key.
2.  **Server Authentication**: The server proves it is who it says it is (Host Key).
3.  **User Authentication**: The user proves who they are (Password, Public Key, Certificate).
4.  **Channel Setup**: Encrypted session begins.

## 12. Trust on First Use (TOFU)

Unlike the web, where we trust a CA (DigiCert), in SSH we typically trust the *key itself*.

*   **First connection**: "The authenticity of host 'github.com' can't be established. ED25519 key fingerprint is SHA256:.... Are you sure?"
*   **You say yes**: The key is saved in `~/.ssh/known_hosts`.
*   **Future connections**: SSH checks if the server's key matches the one in `known_hosts`.

### MITM Risks
If an attacker intercepts your **first** connection, you are owned. You trust the attacker's key. This is why you should verify fingerprints out-of-band (e.g., check GitHub's published fingerprints).

## 13. Ephemeral Session Keys

Just like TLS 1.3, modern SSH uses **Forward Secrecy**.
*   Long-term keys (id_rsa, host keys) are *only* used for authentication.
*   A new temporary (ephemeral) key is generated for every single session to encrypt the data.
*   If someone steals your private key, they can impersonate you in the future, but they generally *cannot* decrypt your past recorded sessions (depending on the KEX algorithm used).

## 14. SSH Certificates

Managing `known_hosts` and `authorized_keys` at scale (10,000 servers, 100 developers) is a nightmare.
Enter **SSH Certificates**.

Instead of copying public keys to every server, you satisfy the server:
"Trust any user key signed by the **Corporate User CA**."

And you satisfy the user:
"Trust any host key signed by the **Corporate Host CA**."

*   **Principals**: "This cert allows logging in as `root` and `ubuntu`."
*   **Validity**: "This cert is valid for 8 hours only." (Great for temporary access).

---

## Labs

- [Lab 3.1: Keys & known_hosts](../labs/module-03/lab-3-1.md)
- [Lab 3.2: Man-in-the-Middle Simulation](../labs/module-03/lab-3-2.md)
- [Lab 3.3: Setting up an SSH CA](../labs/module-03/lab-3-3.md)
