# Lab 9.2: Simulation - Middleware Breakage

**Scenario:** App A calls App B. Fails with "Handshake Failure".

**Investigation:**
1.  Check logs. "no shared cipher".
2.  App A tries to speak TLS 1.0 (Legacy Java 6 app).
3.  App B has upgraded to TLS 1.2+ only.

**Fix:**
Upgrade App A. Or temporarily enable legacy ciphers on App B (Risky!).
