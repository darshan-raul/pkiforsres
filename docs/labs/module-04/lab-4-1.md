# Lab 4.1: TLS Handshake Analysis

**Goal:** Visualize the TLS handshake using Wireshark filters.

## Prerequisites
*   Wireshark installed on your local machine.
*   `curl`

## 1. Capture Traffic

Start Wireshark. Select your main network interface (e.g., WiFi or eth0).
In the filter bar, type:
`tcp.port == 443`

## 2. Generate Traffic

In a terminal:
```bash
curl -v https://example.com
```

## 3. Analyze the Capture

Stop the capture. Look for the stream.

### Locate Client Hello
*   Select the packet marked **Client Hello**.
*   Expand **Transport Layer Security** -> **TLSv1.2 (or 1.3) Record Layer** -> **Handshake Protocol** -> **Client Hello**.
*   Look for **Extension: server_name**. This is SNI. This is how the server knows which cert to serve.

### Locate Server Hello
*   Select **Server Hello**.
*   Look for **Cipher Suite**. Which one did it pick?

### Locate Certificate (TLS 1.2 only)
*   If you used TLS 1.2, you will see a packet usually marked "Certificate, Server Hello Done".
*   You can actually export the certificate object from the packet bytes!

## 4. Filter for Errors

Try to connect to a server that fails (e.g., using `openssl s_client -cipher NULL` if supported, or connecting to a non-ssl port).

Filter: `tls.alert_message.level == 2` (Fatal)

This immediately highlights failed handshakes.
