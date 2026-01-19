# Lab 5.1: OpenVPN Certificates

**Goal:** Inspect OpenVPN PKI structure.

*We won't build a full VPN, but we will inspect the EasyRSA scripts often used.*

1.  **EasyRSA**: A set of shell scripts wrapping openssl to manage a CA.
2.  **Vars**: `set_var EASYRSA_REQ_CN  "OpenVPN CA"`
3.  **Build CA**: `./easyrsa build-ca`
4.  **Gen Req**: `./easyrsa gen-req client1 nopass`
5.  **Sign**: `./easyrsa sign-req client client1`

This is functionally identical to what we did in Lab 2.1, just wrapped in scripts.

**Takeaway:** If you understand Lab 2.1, you understand OpenVPN auth.
