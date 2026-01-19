# Lab 2.4: Cert Rotation

**Goal:** Rotate a certificate without downtime.

## 1. Generate New Cert

The old cert is expiring. We need a new one.

```bash
# New CSR (or reuse old one)
openssl req -new -key leaf/server-key.pem -out leaf/server-new.csr \
    -subj "/C=US/O=MyFakeOrg/CN=mysite.local"

# Sign new cert
openssl x509 -req -in leaf/server-new.csr -CA intermediate/inter-cert.pem -CAkey intermediate/inter-key.pem \
    -CAcreateserial -out leaf/server-cert-new.pem -days 365 \
    -extfile <(echo "subjectAltName=DNS:mysite.local")
```

Create new fullchain:

```bash
cat leaf/server-cert-new.pem intermediate/inter-cert.pem > leaf/fullchain-new.pem
```

## 2. Atomic Swap

In a real server, you shouldn't just overwrite files while the process is reading them.
Move the new file into place.

```bash
cp leaf/fullchain-new.pem leaf/fullchain.pem
```

## 3. Reload Nginx

Nginx loads certs into memory. It won't see the file change until a reload signal.

```bash
docker exec my-nginx nginx -s reload
```

**Verification:**

Check the new expiry date locally or via openssl s_client.

```bash
echo | openssl s_client -connect localhost:8443 2>/dev/null | openssl x509 -noout -dates
```
