# Lab 6.2: DKIM Signing

**Goal:** Generate DKIM keys and DNS records.

## 1. Generate Keys

We use `opendkim-genkey`.

```bash
# install opendkim-tools
opendkim-genkey -s mail -d mycorp.com
```

This creates:
*   `mail.private`: Private key (configure your Postfix server with this).
*   `mail.txt`: The DNS TXT record.

## 2. Inspect DNS Record

```bash
cat mail.txt
```

Output:
`mail._domainkey ... "v=DKIM1; k=rsa; p=MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQ..."`

## 3. Verify

If you had a real domain, you would add this TXT record.
To verify:
```bash
dig mail._domainkey.google.com TXT
```
