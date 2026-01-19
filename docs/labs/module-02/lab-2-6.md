# Lab 2.6: TLS Versions & Performance

**Goal:** Observe the difference between TLS 1.2 and 1.3.

## 1. Force TLS 1.2

```bash
openssl s_client -connect google.com:443 -tls1_2 < /dev/null
```

Output:
*   `Protocol : TLSv1.2`
*   `Cipher : ECDHE-ECDSA-AES128-GCM-SHA256` (example)

## 2. Force TLS 1.3

```bash
openssl s_client -connect google.com:443 -tls1_3 < /dev/null
```

Output:
*   `Protocol : TLSv1.3`
*   `Cipher : TLS_AES_256_GCM_SHA384`

## 3. Identify differences

*   **Handshake messages**: TLS 1.3 encrypts more of the handshake, including the certificate itself! In Wireshark, you can see the certificate in TLS 1.2, but not in 1.3.
*   **Ciphers**: TLS 1.3 removed older, weaker ciphers (CBC mode, RSA key exchange).

## 4. Benchmark (Optional)

You can use `openssl s_time` to benchmark.

```bash
openssl s_time -connect google.com:443 -new -time 5 -tls1_3
```

vs

```bash
openssl s_time -connect google.com:443 -new -time 5 -tls1_2
```
