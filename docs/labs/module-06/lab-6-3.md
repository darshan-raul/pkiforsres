# Lab 6.3: DNSSEC

**Goal:** Validate DNS signatures.

## 1. Dig with +dnssec

Query a secured domain.

```bash
dig +dnssec cloudflare.com
```

Look for the **RRSIG** record in the answer section. That is the signature.

## 2. Check the Chain (CD bit)

You can ask the resolver to *Check Disabled* (CD) to debug.
But mostly, you want to see the AD (Authenticated Data) flag in the header.

```
;; flags: qr rd ra ad; QUERY: 1, ANSWER: 2, AUTHORITY: 0, ADDITIONAL: 1
```

**ad**: Authenticated Data. Your resolver validated the chain up to the root.
