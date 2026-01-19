# Lab 5.2: Database TLS (Postgres)

**Goal:** Enable TLS on Postgres.

## 1. Run Postgres with TLS

We'll use the certs from Lab 2.1.
Postgres expects the key to have strict permissions (0600).

```bash
cp leaf/server-key.pem pg-key.pem
chmod 600 pg-key.pem
cp leaf/server-cert.pem pg-cert.pem
```

Run Docker:

```bash
docker run -d --name my-postgres \
  -e POSTGRES_PASSWORD=mysecretpassword \
  -v $(pwd)/pg-key.pem:/var/lib/postgresql/server.key:ro \
  -v $(pwd)/pg-cert.pem:/var/lib/postgresql/server.crt:ro \
  -d postgres \
  -c ssl=on \
  -c ssl_cert_file=/var/lib/postgresql/server.crt \
  -c ssl_key_file=/var/lib/postgresql/server.key
```

## 2. Connect with Client

Use `psql` (or a docker container with psql).

```bash
# Weak connection (might use SSL but doesn't verify)
psql "postgresql://postgres:mysecretpassword@localhost:5432/postgres?sslmode=require"
```

## 3. Strong Connection (Verify Full)

You need the Root CA.

```bash
psql "postgresql://postgres:mysecretpassword@localhost:5432/postgres?sslmode=verify-full&sslrootcert=root/root-cert.pem"
```

If it connects, you are fully secure and verified.
