# Lab 6.1: SSH with Hardware Keys (YubiKey)

**Goal:** Secure SSH so it requires physical presence.

*Requirements: A YubiKey or similar FIDO2 token.*

## 1. Generate FIDO key (New Standard)

OpenSSH 8.2+ supports FIDO keys directly.

```bash
ssh-keygen -t ed25519-sk -C "yubikey"
```

*   **-sk**: Security Key.
*   It will ask you to touch your token.

## 2. Inspect Key

```bash
cat id_ed25519_sk
```

You will see a "handle" (reference to the hardware key), not the actual private key math. If you copy this file to another machine without the YubiKey, it won't work.

## 3. Use it

Add to agent or just use `-i`.

```bash
ssh -i id_ed25519_sk user@server
```

You must touch the key every time you connect.

## Variation: Resident Keys (`-O resident`)

```bash
ssh-keygen -t ed25519-sk -O resident
```

This stores the key handle *on the YubiKey itself*. You can go to a brand new laptop, plug in your key, run `ssh-add -K`, and boom—you have your keys.
