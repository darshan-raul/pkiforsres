# Lab 7.3: Debugging Istio mTLS

**Goal:** Debug a "upstream connect error" caused by mTLS mismatch.

## Scenario
*   Service A -> Service B.
*   Istio is enabled.

## 1. Check Policy

```bash
kubectl get peerauthentication -A
```

If it says `STRICT`, then Service B **only** accepts mTLS.

## 2. Simulate Failure

If you try to `curl` Service B from a "Legacy Pod" (no sidecar), it will fail.
Why? Because the Legacy Pod speaks plaintext. Service B expects mTLS.

**Fix:**
Either inject sidecar into Legacy Pod OR change Policy to `PERMISSIVE`.

## 3. Inspect Certificate

Istio certs are short-lived.

```bash
istioctl proxy-config secret deploy/my-app -o json | jq '.dynamicActiveSecrets'
```

You can see the certs valid only for 24 hours (or less). This is strong security.
