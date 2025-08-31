

---

### Hash Functions and Message Authentication Codes (MACs)

#### **1. Introduction to Hash Functions**
   - **Purpose**: Hash functions convert variable-length inputs into fixed-size outputs, providing a unique "fingerprint" for data.
   - **Properties**:
     - **One-way**: Difficult to reverse-engineer the original input.
     - **Deterministic**: Same input produces the same output.
   - **Applications**:
     - Digital signatures
     - File integrity checks
     - Password storage
     - Communication integrity

#### **2. Key Hash Function Properties**
   - **Compression**: Reduces input data to a fixed `n`-bit length.
   - **Collision Resistance**: It should be infeasible to find two different inputs with the same hash.
   - **Preimage Resistance**: Given a hash, it’s difficult to find the original input.
   - **Second Preimage Resistance**: Given an input and its hash, finding a different input with the same hash should be infeasible.

#### **3. Types of Hash Functions**
   - **Keyless Hash Functions**: Standard cryptographic hash functions like SHA-1, SHA-256.
   - **Keyed Hash Functions (MACs)**: Used in message authentication (e.g., HMAC).

#### **4. Security Notions for Hash Functions**
   - **Collision Resistance**: Hard to find two messages `M1` and `M2` such that `H(M1) = H(M2)`.
   - **Preimage and Second Preimage Resistance**: Ensures it’s hard to reverse-engineer or replicate hashes.

#### **5. Uses of Hash Functions**
   - **File Integrity**: Hashes detect changes in files by comparing fingerprints.
   - **Password Storage**: Systems store hashed passwords to protect against exposure.
   - **Communication Integrity**: Ensures data hasn’t been tampered with during transmission.
   - **Digital Signatures**: Messages are hashed before signing for secure and efficient verification.

#### **6. MACs (Message Authentication Codes)**
   - **Purpose**: Verifies the authenticity and integrity of a message using a shared secret key.
   - **Construction**:
     - **HMAC (Hash-based MAC)**: Commonly used MAC built from hash functions.
     - **CBC-MAC**: Uses block ciphers to generate MACs in a chain-like structure.

#### **7. Security Attacks on Hash Functions**
   - **Collision Attacks**: Seek two inputs that produce the same hash (affect MD5, SHA-1).
   - **Generic Attacks**:
     - **Collision Resistance**: Naive brute-force attack takes approximately `2^(n/2)` operations.
     - **Preimage Attack**: Takes `2^n` operations.

#### **8. Popular Hash Functions**
   - **MD5**: Developed in 1992 by Rivest. Now considered insecure due to collision vulnerabilities.
   - **SHA Family**:
     - **SHA-1**: Widely used but vulnerable to collision attacks.
     - **SHA-2**: Includes SHA-256, SHA-512, with no known successful attacks.
     - **SHA-3**: Uses sponge construction with a 1600-bit state, resistant to collision and preimage attacks.

#### **9. Hash Function Constructions**
   - **Merkle-Damgård Construction**: Breaks input into fixed-size blocks, uses compression functions, vulnerable to length-extension attacks.
   - **Davies-Meyer Compression Function**: Common construction for hash functions using block ciphers.
   - **Sponge Construction (SHA-3)**: Absorbs input into a large state, then outputs hash after mixing; resistant to certain cryptanalytic attacks.

#### **10. Password Hashing with Argon2**
   - **Purpose**: Slows down brute-force attacks by requiring significant computational resources.
   - **Method**: Hashes the password repeatedly and can demand specific hardware resources to enhance resistance.

#### **11. Security Status of Popular Hash Functions**
   - **MD2, MD4, MD5**: Vulnerable to collision attacks.
   - **SHA-0, SHA-1**: Vulnerable to attacks; SHA-1 is still partially used despite collision issues.
   - **SHA-256, SHA-3, Argon2**: Currently considered secure with no successful attacks.

---
