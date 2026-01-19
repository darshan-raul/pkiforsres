# Lab 2.5: OCSP & Revocation

**Goal:** Understand how to check revocation status.

*Note: Setting up a full OCSP responder is complex. We will simulate the client-side check.*

## 1. Check a real website's OCSP

Let's check `google.com`.

```bash
openssl s_client -connect google.com:443 -status -servername google.com < /dev/null
```

Look for the **OCSP Response** section in the output.

```
OCSP response:
======================================
OCSP Response Data:
    OCSP Response Status: successful (0x0)
    Response Type: Basic OCSP Response
    ...
    Cert Status: good
```

## 2. Stapling

The output above *IS* OCSP stapling. The server sent the OCSP response during the handshake, so we didn't have to ask the CA.

## 3. What if it was revoked?

If `Cert Status: revoked`, the browser should hard stop connection.

**Why it fails in real life:**
Most browsers "soft-fail" on OCSP. If they can't reach the OCSP server, they assume the cert is valid. This is why revocation is considered "broken" by many security experts, and why short-lived certificates (Let's Encrypt 90 days) are preferred over relying on revocation.
