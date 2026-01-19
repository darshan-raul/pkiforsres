# Lab 2.1: Build your own CA

**Goal:** Create a Root CA and an Intermediate CA, then issue a server certificate. This mimics a real organization's PKI.

## 1. Create directory structure

```bash
mkdir -p my-pki/{root,intermediate,leaf}
cd my-pki
```

## 2. Create Root CA

The Root CA is the "Trust Anchor".

```bash
# Generate Root Key
openssl genrsa -out root/root-key.pem 4096

# Create Root Certificate (Self-signed)
openssl req -new -x509 -days 3650 -key root/root-key.pem -out root/root-cert.pem \
    -subj "/C=US/O=MyFakeOrg/CN=MyFakeOrg Root CA"
```

## 3. Create Intermediate CA

We rarely sign with the Root. We use an Intermediate.

```bash
# Generate Intermediate Key
openssl genrsa -out intermediate/inter-key.pem 2048

# Create CSR for Intermediate
openssl req -new -key intermediate/inter-key.pem -out intermediate/inter.csr \
    -subj "/C=US/O=MyFakeOrg/CN=MyFakeOrg Intermediate CA"

# Sign Intermediate CSR with Root Key
# Note: We need a config file to set "CA:TRUE" extension, or use -addext in newer openssl
openssl x509 -req -in intermediate/inter.csr -CA root/root-cert.pem -CAkey root/root-key.pem \
    -CAcreateserial -out intermediate/inter-cert.pem -days 1825 \
    -extensions v3_ca -extfile <(echo "[v3_ca]"; echo "basicConstraints=critical,CA:TRUE")
```

## 4. Verify Chain

```bash
openssl verify -CAfile root/root-cert.pem intermediate/inter-cert.pem
```

## 5. Issue Server Certificate

Now issue a cert for `mysite.local`.

```bash
# Generate Server Key
openssl genrsa -out leaf/server-key.pem 2048

# Create CSR
openssl req -new -key leaf/server-key.pem -out leaf/server.csr \
    -subj "/C=US/O=MyFakeOrg/CN=mysite.local"

# Sign with Intermediate
openssl x509 -req -in leaf/server.csr -CA intermediate/inter-cert.pem -CAkey intermediate/inter-key.pem \
    -CAcreateserial -out leaf/server-cert.pem -days 365 \
    -extfile <(echo "subjectAltName=DNS:mysite.local,DNS:www.mysite.local")
```

## 6. Create Full Chain

Servers need the certificate + intermediate.

```bash
cat leaf/server-cert.pem intermediate/inter-cert.pem > leaf/fullchain.pem
```

You now have a production-ready certificate chain!
