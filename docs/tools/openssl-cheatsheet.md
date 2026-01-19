# OpenSSL Cheatsheet

## Checking Things

**Check a remote server:**
```bash
openssl s_client -connect google.com:443
```

**Check a Certificate File (PEM):**
```bash
openssl x509 -in cert.pem -text -noout
```

**Check a Private Key:**
```bash
openssl rsa -in key.pem -check
```

**Check a CSR:**
```bash
openssl req -in request.csr -text -noout
```

## Conversions

**PEM to DER:**
```bash
openssl x509 -in cert.pem -outform der -out cert.der
```

**PFX/P12 to PEM:**
```bash
openssl pkcs12 -in bundle.pfx -out bundle.pem -nodes
```

## Generation

**New Private Key:**
```bash
openssl genrsa -out key.pem 2048
```

**New CSR (from existing key):**
```bash
openssl req -new -key key.pem -out request.csr
```

**Self-Signed Cert:**
```bash
openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365
```
