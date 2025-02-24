### SSL/TLS Overview

---

#### **What is SSL/TLS?**

- Protocols for securing communication over the Internet.
- **SSL (Secure Sockets Layer)**: Predecessor of TLS.
- **TLS (Transport Layer Security)**: Modern, IETF-maintained protocol.
- Operates over TCP and provides:
    1. **Authentication**: Verify server identity (and optionally client).
    2. **Confidentiality**: Encrypt communication.
    3. **Integrity**: Prevent data tampering.

---

#### **TLS History**

1. **SSL**:
    - SSL 1.0: Not released.
    - SSL 2.0 (1995): Initial version (deprecated).
    - SSL 3.0 (1996): Foundation for TLS.
2. **TLS Versions**:
    - TLS 1.0 (1999): Enhanced security.
    - TLS 1.1 (2006): Added protections.
    - TLS 1.2 (2008): Widely adopted.
    - TLS 1.3 (2018): Simplified handshake, stronger encryption.

**Current Adoption**:

- TLS 1.2: 99.9%.
- TLS 1.3: 66.9% of HTTPS servers.

---

#### **TLS Cryptographic Mechanisms**

1. **Authentication**:
    - Public-key cryptography:
        - RSA, DSS, ECDSA.
    - Pre-shared keys (PSK) for lightweight connections.
2. **Key Exchange**:
    - RSA: Uses a static key pair.
    - Diffie-Hellman (DH/ECDH): Static or ephemeral.
    - Ephemeral DH (DHE/ECDHE): Ensures forward secrecy.
3. **Encryption**:
    - Block ciphers: AES-CBC, 3DES.
    - Stream ciphers: RC4 (deprecated).
    - AEAD: AES-GCM for authenticated encryption.
4. **Message Integrity**:
    - HMAC: SHA-256, SHA-384.

---

#### **TLS Cipher Suites**

- Combination of:
    1. Key exchange algorithm.
    2. Authentication method.
    3. Encryption algorithm.
    4. Hash function.
- Example: `TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256`.

---

#### **Handshake Process**

1. **ClientHello**:
    - Proposes supported cipher suites and highest protocol version.
2. **ServerHello**:
    - Selects cipher suite and protocol version.
    - Sends its certificate (X.509) for authentication.
3. **Key Exchange**:
    - Client and server exchange keys (e.g., via Diffie-Hellman).
4. **Finished**:
    - Both parties compute session keys and verify the handshake.

---

#### **Public Key Infrastructure (PKI)**

- Ensures trust in public keys via certificates.

1. **Certificate Authority (CA)**:
    - Issues signed certificates.
    - Root CAs certify intermediate CAs.
2. **Certificate Validation**:
    - Server sends a certificate chain.
    - Client verifies using a trusted root CA.

**Challenges**:

- CA compromise can issue fraudulent certificates (e.g., DigiNotar breach).

---

#### **Common Vulnerabilities**

1. **Legacy Protocols**:
    - SSL 3.0 (POODLE attack).
    - TLS 1.0/1.1 vulnerabilities.
2. **Implementation Flaws**:
    - Heartbleed (OpenSSL memory leak).
    - CRIME (compression-related leakage).
    - BEAST (CBC attack).
3. **Weak Ciphers**:
    - RC4 attacks.
    - Export-grade cryptography (Logjam).

---

#### **TLS 1.3 Improvements**

- Removed vulnerable features:
    - RSA key exchange.
    - Static Diffie-Hellman.
- Streamlined handshake:
    - Reduced to one round trip.
    - All exchanges encrypted after initial key agreement.

---
