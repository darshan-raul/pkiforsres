# Lab 7.2: Manual Certificate Rotation

**Goal:** Understand how risky manual rotation is.

*Warning: Do not do this on a prod cluster. Use Minikube.*

## 1. Check current expiry

```bash
kubeadm certs check-expiration
```

## 2. Backup!

```bash
cp -r /etc/kubernetes/pki /etc/kubernetes/pki-backup
```

## 3. Renew

```bash
kubeadm certs renew all
```

## 4. Restart services

Just renewing the file isn't enough. The processes are holding the old specific file descriptors (or have cached content).
You must restart the static pods.

```bash
mv /etc/kubernetes/manifests/*.yaml /tmp/
sleep 20
mv /tmp/*.yaml /etc/kubernetes/manifests/
```

This kills and restarts the Control Plane.
