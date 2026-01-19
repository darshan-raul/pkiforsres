# Outage Scenario: The Expired Certificate

**Symptoms:**
*   Users see red warning screen in browser "NET::ERR_CERT_DATE_INVALID".
*   Backend services fail with "x509: certificate has expired".

**Resolution:**
1.  **Immediate**: Issue a new certificate using your CA (or ACME).
2.  **Deploy**: Reload the web server / load balancer.
3.  **Verify**: `openssl s_client ... | openssl x509 -dates`.

**Prevention**:
*   Prometheus Blackbox Exporter monitoring expiry.
*   Alert at 30 days remaining.
*   Use Cert-Manager for auto-renewal.
