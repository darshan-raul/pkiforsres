# Outage Scenario: The mTLS Break

**Symptoms:**
*   Services return 503 or Connection Reset.
*   Logs say "bad certificate", "unknown ca", or "certificate required".

**Common Causes:**
1.  **Client forgot cert**: Sending no cert to a server that requires it.
2.  **Wrong CA**: Client cert signed by Dev CA, Server trusts Prod CA.
3.  **Missing SAN**: Client cert has IP 1.2.3.4, Server expects DNS 'my-service'.

**Resolution:**
1.  Inspect traffic with `tcpdump` to see if client sends a cert.
2.  Decode client cert and check Issuer.
