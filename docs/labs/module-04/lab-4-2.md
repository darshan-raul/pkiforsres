# Lab 4.2: SSH Handshake Analysis

**Goal:** Compare SSH handshake to TLS.

## 1. Capture Traffic

Filter Wireshark: `tcp.port == 22` (or your lab port).

## 2. Generate Traffic

```bash
ssh user@host echo hello
```

## 3. Analyze

*   **Protocol version exchange**: Both sides send a cleartext string like `SSH-2.0-OpenSSH_8.9`.
*   **Key Exchange Init**: They agree on algorithms (KEX, Host Key algo, Encryption algo, MAC algo).
*   **Encrypted packet**: Suddenly, the protocol column just says "SSHv2" and "Encrypted packet". You can't see the password or the command "hello".

## 4. Side Channel: Timing

Type keys slowly in an interactive SSH session.
Observe the Wireshark capture.
You will see 1 packet going out, 1 packet coming back (echo).
*   **Fact**: In older SSH configurations, you could guess password lengths or command lengths by packet size/timing.
*   **Mitigation**: Modern SSH uses padding to obscure exact lengths.
