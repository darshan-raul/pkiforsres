# Lab 9.1: Simulation - The Expired Cert

**Scenario:** Web server is down. Users see "SEC_ERROR_EXPIRED_CERTIFICATE".

**Investigation:**
1.  `echo | openssl s_client -connect localhost:8443 | openssl x509 -noout -enddate`
2.  Output: `notAfter=Jan 19 00:00:00 2020 GMT`
3.  **Root Cause**: It expired years ago.

**Fix:**
Revisit Lab 2.4 (Rotation).
