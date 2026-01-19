# Module 6: Identity & Passwordless

**Goal:** Using PKI to kill the password.

## 20. Passwordless Authentication

Passwords are bad. Humans are bad at them. Computers are great at keys.

### FIDO2 / WebAuthn
*   **The Standard**: Allows you to use a hardware token (YabiKey) or platform auth (TouchID/Windows Hello) to log in to websites.
*   **PKI under the hood**:
    1.  **Registration**: Your device creates a Key Pair. Sends Public Key to Google.
    2.  **Login**: Google sends a challenge. Your device signs it with Private Key (after you touch it).
    3.  **Google verifies**: You are logged in.

### Smart Cards / YubiKeys (PIV)
*   **PIV (Personal Identity Verification)**: A standard for storing keys on smart cards.
*   **SSH**: You can store your SSH private key *inside* the YubiKey. It cannot be extracted. To use it, you must plug it in and touch it.

## 21. Email Security

Email is inherently insecure (plaintext). PKI fixes it.

*   **DKIM (DomainKeys Identified Mail)**:
    *   Server signs the email headers with a Private Key.
    *   Receiver gets email, looks up Public Key in DNS TXT record.
    *   Verifies signature.
    *   Pass: "This email definitely came from google.com".
*   **S/MIME**: Encrypting the body of the email itself (End-to-End). Hard to use, enterprise only.

## 22. DNS Security (DNSSEC)

DNS is the phonebook. If I poison the phonebook, I can send you to a fake bank server.
*   **DNSSEC**: Signs the DNS records.
*   **Chain**: Root (.) signs `.com`, `.com` signs `google.com`.
*   **Validation**: Your resolver verifies the chain.

---

## Labs

- [Lab 6.1: SSH with Hardware Keys](../labs/module-06/lab-6-1.md)
- [Lab 6.2: DKIM Signing](../labs/module-06/lab-6-2.md)
- [Lab 6.3: DNSSEC](../labs/module-06/lab-6-3.md)
