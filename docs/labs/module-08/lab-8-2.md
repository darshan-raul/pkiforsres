# Lab 8.2: HSM Simulation

**Goal:** Understand `pkcs11`.

*Note: Requires `softhsm2` package.*

1.  **Initialize Token**:
    `softhsm2-util --init-token --slot 0 --label "MyToken"`
2.  **Generate Key in Hardware**:
    `pkcs11-tool --keypairgen --key-type rsa:2048 --label "mykey"`
3.  **Sign with Hardware**:
    `openssl` can use an engine (`-engine pkcs11`) to offload the math to the token.

**Takeaway:** The private key file never touches the disk. It stays inside the "hardware" (SoftHSM in this case).
