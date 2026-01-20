# pkiforsres

![alt text](images/pkiforsresbanner.png)

>Ever felt overwhelmed by acronyms like X.509, CRL, SAN, PKCS, and cipher suites?

> Ever copy-pasted an OpenSSL command from Stack Overflow, hoping it would “just work”?

> Whats RSA vs ECDSA?

> You’re not alone. PKI is one of those things everyone uses, few fully understand, and almost everyone gets burned by at least once.

> This guide is your practical, hands-on path to finally understanding PKI—from first principles to real-world debugging.

Welcome to **PKI for SREs**, a guide designed specifically for SREs/Cloud Engineers/DevOps Engineers who need to understand, operate, and debug Public Key Infrastructure in real-world environments.

Although I will try to make all the theory ELI5 and explain with analogies and explain the practical implementations of each concept. Finally we will also have labs to solidify the learning.

We will go deep and cover all the possible concepts for you to understand PKI in production. I will mark some sections as *optional* if they are too complex and not required for day to day operations.

Roadmap

- [ ] simple public private keys
- [ ] understand the different algorithms and their purpose
- [ ] explain tls and origins
- [ ] show complete public cert flow
- [ ] explain all the cipher suites and their meaning
- [ ] cover root ca hierarchy and bridge/mesh model 
- [ ] explain private ca and private certs
- [ ] explain self signed certs
- [ ] explain ssh flow 
- [ ] explain ssh access with certs and at scale
- [ ] explain mTLS
- [ ] explain digital signature
- [ ] explain mtls with a docker example
- [ ] show mtls in a kind cluster