# Lab 7.1: Inspecting Kubernetes Certs

**Goal:** See the hidden PKI inside K8s.

*Prerequisite: Access to a K8s control plane (minikube, k3d, or kind).*

## 1. Where are the keys?

On a kubeadm/standard cluster:

```bash
ls -l /etc/kubernetes/pki/
```

You will see:
*   `ca.crt`, `ca.key`: The Master Root.
*   `apiserver.crt`, `apiserver.key`: Keys for the API.
*   `etcd/ca.crt`: Separate CA for Etcd.

## 2. Decode the API Server Cert

```bash
openssl x509 -in /etc/kubernetes/pki/apiserver.crt -text -noout
```

**Check the SANs**:
You will see:
`DNS:kubernetes`, `DNS:kubernetes.default`, `DNS:kubernetes.default.svc`, `IP:10.96.0.1`, `IP:192.168.x.x`

This is why you can talk to the API server from inside pods (`kubernetes.default`) and outside (`192.168...`).

## 3. Check Kubeconfig

Look at your local `~/.kube/config`.

```bash
cat ~/.kube/config
```

It contains `client-certificate-data` and `client-key-data` (base64 encoded).
This represents YOU (kubernetes-admin) authenticating to the cluster via mTLS.
