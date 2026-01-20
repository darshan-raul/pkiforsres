# PKI for SREs 

![alt text](../images/pkiforsresbanner.png)

>Ever felt overwhelmed by acronyms like X.509, CRL, SAN, PKCS, and cipher suites?

> Ever copy-pasted an OpenSSL command from Stack Overflow, hoping it would “just work”?

> Whats RSA vs ECDSA?

> You’re not alone. PKI is one of those things everyone uses, few fully understand, and almost everyone gets burned by at least once.

> This guide is your practical, hands-on path to finally understanding PKI—from first principles to real-world debugging.

Welcome to **PKI for SREs**, a guide designed specifically for SREs/Cloud Engineers/DevOps Engineers who need to understand, operate, and debug Public Key Infrastructure in real-world environments.

Although I will try to make all the theory ELI5 and explain with analogies and explain the practical implementations of each concept. Finally we will also have labs to solidify the learning.

We will go deep and cover all the possible concepts for you to understand PKI in production. I will mark some sections as *optional* if they are too complex and not required for day to day operations.

## Guide Structure

The guide is divided into 10 modules, taking you from the absolute basics to advanced topics like Zero Trust and mTLS in Kubernetes.

| Module | Topic | Focus |
| :--- | :--- | :--- |
| **Module 1** | [Foundations of PKI](module-01/index.md) | Mental models, crypto landscape, signing vs encryption |
| **Module 2** | [TLS & HTTPS Deep Dive](module-02/index.md) | Handshakes, certificates, rotation, operational reality |
| **Module 3** | [SSH PKI](module-03/index.md) | SSH keys, CAs, TOFU, and enterprise SSH access |
| **Module 4** | [Packet-Level Visibility](module-04/index.md) | Wireshark, tcpdump, and seeing crypto on the wire |
| **Module 5** | [PKI Across Infra](module-05/index.md) | VPNs, Database TLS, and other protocols |
| **Module 6** | [Identity & Passwordless](module-06/index.md) | FIDO2, YubiKeys, Email security, DNSSEC |
| **Module 7** | [mTLS & Kubernetes](module-07/index.md) | Client auth, Zero Trust, K8s internal PKI, Service Mesh |
| **Module 8** | [PKI Operations](module-08/index.md) | Lifecycle management, offline roots, HSMs |
| **Module 9** | [Troubleshooting & On-call](module-09/index.md) | Debugging playbooks, common outages, incident response |
| **Module 10** | [Zero Trust & Modern PKI](module-10/index.md) | SPIFFE, ACME, Future of PKI |

## Why I created this

- I have read a lot of articles and blogs on PKI over the period. Excellent ones! and you will find them in the notebookLM in the sources section. 
- But as a beginner I would have always craved for a single place where I can map all of those concepts and understand them practically. How to view it in aws? in a nginx server? in a k8s cluster? during ssh sessions?
- This is that attempt from me for the next lot of beginners. 
- **Enjoy and Be curious!**


## Prerequisites

*   Basic Linux command line knowledge.
*   Basic understanding of networking (IPs, ports, DNS).
*   A Linux machine is encouraged, but windows [with WSL] and macs are also supported.