# Module 9: Troubleshooting & On-call

**Goal:** Fix it fast.

## 29. Debugging Playbook

1.  **Check Expiry**: `openssl x509 -enddate -noout -in cert.pem`
2.  **Check Chain**: `openssl s_client -connect host:443 -showcerts`. look for errors 20 (unable to get local issuer certificate).
3.  **Check SNI**: Are you sending the right hostname? `-servername example.com`.
4.  **Check Port**: Is it actually TLS? `nc -v host 443`.

## 30. Common Outages

*   **The "Monday Morning" Outage**: Cronjob runs on Sunday to renew cert, but Nginx wasn't reloaded.
*   **The "Vendor" Outage**: An external API changed their intermediate CA, and you hardcoded the old one.
*   **The "IoT" Outage**: You shipped 1,000 devices with a cert that expires in 1 year. They are now bricks.

---

## Labs

- [Lab 9.1: Simulation - The Expired Cert](../labs/module-09/lab-9-1.md)
- [Lab 9.2: Simulation - Middleware Breakage](../labs/module-09/lab-9-2.md)
