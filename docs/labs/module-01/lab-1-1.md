# Lab 1.1: Generate RSA vs ECDSA Keys

**Goal:** Get hands-on with `openssl` to generate different types of keys and compare them.

## Prerequisites
*   Terminal with `openssl` installed.

## Step 1: Generate an RSA Key

Generate a standard 2048-bit RSA private key.

```bash
openssl genrsa -out rsa-key.pem 2048
```

Inspect the key content:

```bash
cat rsa-key.pem
```

View the detailed structure (primes, exponents):

```bash
openssl rsa -in rsa-key.pem -text -noout
```

## Step 2: Generate an ECDSA Key

Generate a Prime256v1 (standard NIST curve) private key.

```bash
openssl ecparam -name prime256v1 -genkey -noout -out ecdsa-key.pem
```

Inspect the key. Notice how much smaller it is than the RSA key:

```bash
cat ecdsa-key.pem
```

View details:

```bash
openssl ec -in ecdsa-key.pem -text -noout
```

## Step 3: Compare Size

Compare the file sizes.

```bash
ls -l rsa-key.pem ecdsa-key.pem
```

**Observation:**
*   RSA key is significantly larger.
*   ECDSA key provides equivalent or better security with a fraction of the bits. This matters for handshake performance on mobile devices and IoT.

## Step 4: Extract Public Keys

Extract the public key from the private key. This is what you would share with the world.

**RSA:**
```bash
openssl rsa -in rsa-key.pem -pubout -out rsa-public.pem
cat rsa-public.pem
```

**ECDSA:**
```bash
openssl ec -in ecdsa-key.pem -pubout -out ecdsa-public.pem
cat ecdsa-public.pem
```
