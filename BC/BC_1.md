## Lesson 1: Introduction to Cryptography

### Goals of Cryptography
- **Primary Goal**: Secure communication over public channels with adversaries present.
  - **Passive Adversary**: Can only listen.
  - **Active Adversary**: Can modify, write, or erase information.

### Security Services
1. **Confidentiality**: Ensure information is only accessible to authorized parties (e.g., Encryption for GSM, Internet).
2. **Integrity**: Ensure that data has not been altered (e.g., Message Authentication Code, Digital Signatures).
3. **Authentication**: Verify the origin or identity of data (e.g., MAC, Signatures).
4. **Non-repudiation**: Prevent a signer from denying the validity of their signature.

## History of Cryptography
1. **Early (1900 - 1970)**:
   - **Caesar Cipher**: Simple substitution (shift cipher).
   - **Enigma Machine**: Mechanical encryption used in WWII.

2. **Modern Cryptography**:
   - Address key challenges like secure key exchange over public channels (Diffie-Hellman) and authentication without revealing secrets (zero-knowledge protocols).

## Cryptography vs Cryptanalysis
- **Cryptography**: The art of designing secure communication methods.
- **Cryptanalysis**: The study of breaking cryptographic algorithms, finding vulnerabilities.

## Types of Cryptography
1. **Secret-Key Cryptography (Symmetric)**:
   - Both encryption and decryption use the same key.
   - Example: Block ciphers like DES, AES.
   - **Security**: Cannot recover plaintext without the key.

2. **Public-Key Cryptography (Asymmetric)**:
   - Involves two keys: a public key for encryption and a private key for decryption.
   - Example: RSA, Diffie-Hellman.
   - **Security**: Ciphertext can only be decrypted by the holder of the private key.

## Kerckhoffs' Principle
- The security of a cryptographic system should depend solely on the secrecy of the key, not the algorithm.
- Only a small portion of data (the key) needs to be secret.

## Security Levels and Computation Time
- A computer at 1 GHz can perform approximately $(2^{30})$ operations per second.
- **High Security Levels**:
  - Symmetric-Key: 128-256 bits.
  - Public-Key: RSA (2048 bits), elliptic curves (256 bits).
  - Operations like $(2^{128})$ are considered infeasible within a reasonable time frame.

## Encryption Types
1. **Secret-Key Encryption**:
   - Shared secret key used for both encryption and decryption.
   - Example: AES.
   
2. **Public-Key Encryption**:
   - Uses a public key to encrypt and a private key to decrypt.
   - Example: RSA, ElGamal.

### Hybrid Encryption
- Combines public-key cryptography for securely exchanging a **session key**, and symmetric cryptography (e.g., AES) for fast encryption of the actual message.
  
  Example process:
  1. Encrypt session key with RSA public key.
  2. Encrypt the message using AES with the session key.

## One-Time Pad
- **Invented by Vernam (1917)**: Provides perfect secrecy if used correctly.
- Encryption: \( E(m, k) = m \oplus k \)
- Decryption: \( D(c, k) = c \oplus k \)
- **Drawback**: The key must be as long as the message and cannot be reused.

## Certificates and Public Key Infrastructure (PKI)
- **Certificates**: Link between an identity and a public key, validated by trusted Certificate Authorities (CAs).
- Important fields in certificates: 
  - Issuer, Validity period, Public Key, Identity.

## Conclusion
- **Cryptography**: Science of securing communication in digital form.
- Guarantees three major security services:
  - **Confidentiality**
  - **Integrity**
  - **Authentication**

##### Recommended Reading (biblio du cours)
1. **Books**:
   - "The Code Breakers" - David Kahn
   - "The Code Book" - Simon Singh
   - "Applied Cryptography" - Bruce Schneier
2. **Online Resources**:
   - Handbook of Applied Cryptography: [www.cacr.math.uwaterloo.ca/hac](http://www.cacr.math.uwaterloo.ca/hac)
