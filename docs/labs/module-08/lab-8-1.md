# Lab 8.1: Hashicorp Vault PKI Engine

**Goal:** Automate issuance with Vault.

*Prerequisites: Vault installed.*

## 1. Start Vault Dev

```bash
vault server -dev -dev-root-token-id=root
```

## 2. Enable PKI Engine

```bash
export VAULT_ADDR='http://127.0.0.1:8200'
export VAULT_TOKEN=root

vault secrets enable pki
```

## 3. Generate Root CA (in Vault)

```bash
vault write pki/root/generate/internal \
    common_name=my-vault-root \
    ttl=87600h
```

## 4. Configure Role

A role defines what you can issue.

```bash
vault write pki/roles/example-dot-com \
    allowed_domains=example.com \
    allow_subdomains=true \
    max_ttl=72h
```

## 5. Issue Cert

```bash
vault write pki/issue/example-dot-com \
    common_name=www.example.com
```

**Result:** Vault returns the Key and Cert immediately.
**Ops Life**: Your CI/CD pipeline does this, not you manually.
