# Lab 5.3: SMTP StartTLS

**Goal:** Test email security.

Connect to Gmail's SMTP server and check TLS.

```bash
openssl s_client -starttls smtp -connect smtp.gmail.com:587
```

Observe:
1.  Connects in plaintext.
2.  Sends `STARTTLS` command.
3.  Upgrades to TLS (Handshake).
4.  Shows Certificate chain (Google Trust Services).

This "Upgrade" pattern is common in legacy protocols but slightly dangerous if the initial handshake is intercepted.
