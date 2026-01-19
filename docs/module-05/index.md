# Module 5: PKI Across Infra

**Goal:** It's not just for websites. PKI is everywhere.

## 17. VPN and PKI

### OpenVPN
*   Uses a custom TLS implementation.
*   **Process**:
    1.  Client has `ca.crt`, `client.crt`, `client.key`.
    2.  Server has `ca.crt`, `server.crt`, `server.key`, `dh.pem`.
    3.  Mutual TLS (mTLS) handshake occurs locally.
    4.  Tunnel is established.

### WireGuard
*   **Not PKI**. It's "Crypto Routing".
*   No standard CA. No chains.
*   Just **Public Key A** talks to **Public Key B**.
*   Simple, fast, but harder to manage identity at scale without an overlay (like Tailscale).

## 18. Database TLS

Databases are notorious for unencrypted traffic by default.

*   **MySQL**: Often requires generating certs manually and putting them in `/var/lib/mysql`.
*   **Postgres**: `ssl = on` in `postgresql.conf`.
*   **Issues**:
    *   Drivers (Java, Python) often default to `sslmode=prefer` (Opportunistic). Meaning if an attacker blocks port 5432 TLS, the client silently downgrades to plaintext. **Bad.**
    *   **Always use**: `sslmode=verify-full` (Check cert AND hostname).

## 19. Other Protocols

*   **ftps/imaps/smtps**: Older protocols wrapped in TLS (StartTLS).
    *   **StartTLS**: "Start in plaintext, then upgrade". Dangerous because an attacker can strip the upgrade command.

---

## Labs

- [Lab 5.1: OpenVPN Certificates](../labs/module-05/lab-5-1.md)
- [Lab 5.2: Database TLS (Postgres)](../labs/module-05/lab-5-2.md)
- [Lab 5.3: SMTP StartTLS](../labs/module-05/lab-5-3.md)
