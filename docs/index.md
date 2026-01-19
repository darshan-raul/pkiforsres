# PKI in Production – An SRE Survival Guide

Welcome to **PKI in Production**, a course designed specifically for Site Reliability Engineers who need to understand, operate, and debug Public Key Infrastructure in real-world environments.

This course moves beyond the theoretical mathematics of cryptography and focuses on the **practical, operational, and "on-the-wire" reality** of PKI.

## Course Structure

The course is divided into 10 modules, taking you from the absolute basics to advanced topics like Zero Trust and mTLS in Kubernetes.

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

## How to use this course

1.  **Read the Concepts**: Each module starts with a deep dive into the core concepts.
2.  **Do the Labs**: This is a hands-on course. You will generate keys, break certificates, spy on traffic, and fix broken systems.
3.  **Use the Cheatsheets**: Keep the [OpenSSL Cheatsheet](tools/openssl-cheatsheet.md) handy.

## Prerequisites

*   Basic Linux command line proficiency.
*   Basic understanding of networking (IPs, ports, DNS).
*   A Linux environment (VM, WSL, or native) with `openssl` installed.
