# Lab 1.2: Digital Signatures

**Goal:** Understand signing and verification by manually signing a file.

## Prerequisites
*   Completed Lab 1.1 (You need the keys).

## Step 1: Create a document

Create a simple text file.

```bash
echo "This is a critical configuration file." > config.txt
```

## Step 2: Sign the file

We will use our **Private Key** to sign the file. We are using the `dgst` command to hash the file (sha256) and then sign the hash.

**Using RSA Key:**

```bash
openssl dgst -sha256 -sign rsa-key.pem -out config.txt.sig config.txt
```

This creates `config.txt.sig`, which is the binary signature.

## Step 3: Verify the signature

Now, pretend you are the receiver. You have the file `config.txt`, the signature `config.txt.sig`, and the sender's **Public Key** `rsa-public.pem`.

```bash
openssl dgst -sha256 -verify rsa-public.pem -signature config.txt.sig config.txt
```

**Output:**
> Verified OK

## Step 4: Tamper with the file

Modify the file slightly.

```bash
echo "HACKED" >> config.txt
```

Now try to verify the signature again.

```bash
openssl dgst -sha256 -verify rsa-public.pem -signature config.txt.sig config.txt
```

**Output:**
> Verification Failure

**Takeaway:** This is exactly how Secure Boot, JWTs, and Certificate Authorities work. If even one bit changes, the signature is invalid.
