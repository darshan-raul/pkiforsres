# Lab 2.3: Break TLS

**Goal:** Intentionally misconfigure certificates to observe common SRE error messages.

## 1. Wrong Hostname (SAN Mismatch)

Try to access the server using a name not in the SAN (Subject Alternative Name).
Our cert has `mysite.local`. Let's try `wrong.local`.

```bash
curl -v --cacert root/root-cert.pem --resolve wrong.local:8443:127.0.0.1 https://wrong.local:8443
```

**Error:** `SSL: no alternative certificate subject name matches target host name 'wrong.local'`

## 2. Expired Certificate

Generate a certificate that expired yesterday.

```bash
openssl x509 -req -in leaf/server.csr -CA intermediate/inter-cert.pem -CAkey intermediate/inter-key.pem \
    -CAcreateserial -out leaf/expired-cert.pem -days -1 \
    -extfile <(echo "subjectAltName=DNS:mysite.local")
```

Wait, `openssl` might not accept negative days directly in older versions to set enddate in past easily without `faketime` or `-enddate`.
Alternative: Set start date in the deep past and end date in the past.

Easier way: just update nginx to use this new cert (we'll assume you managed to make an expired one, or set system time forward).

But for this lab, let's look at the client side check.

```bash
# Verify the expired cert against the CA
openssl verify -CAfile root/root-cert.pem leaf/expired-cert.pem
```

**Error:** `error 10 at 0 depth lookup: certificate has expired`

## 3. Missing Intermediate

Configure Nginx to use `server-cert.pem` instead of `fullchain.pem`.

Edit `nginx.conf`:
```nginx
ssl_certificate /etc/nginx/certs/server-cert.pem; # changing from fullchain
```

Reload nginx container.

```bash
docker exec my-nginx nginx -s reload
```

Now curl. It *might* work if the client has cached the intermediate, but often it fails or requires AIA fetching.
Use `openssl s_client` to see the chain.

```bash
openssl s_client -connect localhost:8443 -servername mysite.local
```

**Observation:** You will see only *Certificate chain: 0 s: ...* and no intermediate. This causes "incomplete chain" errors on many clients (Android, Java).
