# Module 7: mTLS & Kubernetes

**Goal:** The SRE's Specialty. Zero Trust in the cluster.

## 23. mTLS Fundamentals

**Mutual TLS (mTLS)**:
*   **Normal TLS**: Client verifies Server.
*   **mTLS**: Client verifies Server AND Server verifies Client.

**Why?**
In a microservices world (Service A -> Service B), how do we know it's really Service A calling? IP addresses can be spoofed. Certificates cannot.

## 24. Kubernetes PKI

Kubernetes is just a bunch of binaries talking to each other using mTLS.

*   **Root CA**: `/etc/kubernetes/pki/ca.crt`. Everything trusts this.
*   **API Server**: Has a cert signed by Root CA.
*   **Kubelet**: Has a cert. Authenticates to API Server.
*   **Etcd**: Has its own generic CA usually. Stores the secrets.

**The Danger**: If you lose the CA key, you lose the cluster. You cannot re-sign components. You must rebuild.

## 25. Service Mesh (Istio/Linkerd)

Managing mTLS between 500 microservices manually is impossible.
Enter **Service Mesh**.

-   **Sidecar (Envoy)**: Runs next to your app.
-   **Control Plane (istiod)**: Acts as a CA. Issues short-lived (1 hr) certs to every sidecar.
-   **Traffic**: App A talks plaintext to local Envoy -> Envoy encrypts (mTLS) -> Remote Envoy decrypts -> App B.
-   **Result**: Automatic, rotated, zero-trust mTLS without changing application code.

---

## Labs

- [Lab 7.1: Inspecting Kubernetes Certs](../labs/module-07/lab-7-1.md)
- [Lab 7.2: Manual Certificate Rotation (The Hard Way)](../labs/module-07/lab-7-2.md)
- [Lab 7.3: Debugging Istio mTLS](../labs/module-07/lab-7-3.md)
