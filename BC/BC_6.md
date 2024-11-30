
# RSA and Related Concepts

---

## Modular Arithmetic and Euler’s Theorem

1. **Definitions**:
   - $N = pq$, the product of two large primes.
   - $(\mathbb{Z}/N\mathbb{Z})^* = \{ 1 \leq k < N : \gcd(k, N) = 1 \}$ represents the set of invertible elements modulo $N$.
   - $\phi(N) = (p-1)(q-1)$, the number of invertible elements modulo $N$.

2. **Euler’s Theorem**:
   - For any $a \in (\mathbb{Z}/N\mathbb{Z})^*$:
     $$
     a^{\phi(N)} \equiv 1 \pmod{N}
     $$
   - The exponent can be reduced modulo $\phi(N)$ when computing $a^k \mod N$.

---

## RSA Key Generation

1. Generate two large primes $p$ and $q$ (e.g., 2048 bits).
2. Compute $N = pq$ and $\phi(N) = (p-1)(q-1)$.
3. Choose a public exponent $e$, typically $2^{16} + 1$.
4. Compute the private key $d$ such that:
   $$
   ed \equiv 1 \pmod{\phi(N)}
   $$
   This can be done using the Extended Euclidean Algorithm.
5. Public key: $(e, N)$.
6. Private key: $(d, N)$.

---

## RSA Encryption and Decryption

1. **Encryption**:
   - For a plaintext $M$, compute:
     $$
     c = M^e \mod N
     $$

2. **Decryption**:
   - For a ciphertext $c$, compute:
     $$
     M = c^d \mod N
     $$

3. **Why It Works**:
   - By Euler’s theorem:
     $$
     c^d = (M^e)^d = M^{ed} \equiv M \pmod{N}
     $$

4. **Efficiency**:
   - Encryption is faster (small $e$), while decryption (large $d$) is slower.

---

## Fast Modular Exponentiation

Use the **Square-and-Multiply Algorithm** for efficient computation:
```python
def expmod(C, e, N):
    P = 1
    S = C
    while e > 0:
        if e % 2 == 1:
            P = (P * S) % N
        S = (S * S) % N
        e //= 2
    return P
```

- Complexity: $O(\log(e) \cdot \log^2(N))$.

---

## RSA Signatures

1. **Hash-and-Sign Paradigm**:
   - Signature generation:
     $$
     s = H(M)^d \mod N
     $$
   - Verification:
     $$
     s^e \mod N \stackrel{?}{=} H(M)
     $$
   - Signatures are compact and depend on the hash of the message.

2. **Efficiency**:
   - Signature generation is slow (large $d$).
   - Verification is fast (small $e$).

---

## Common Attacks on RSA

1. **Small Public Exponent ($e = 3$)**:
   - If the message $M$ is small and $M^e < N$, the ciphertext can be directly decrypted without modular reduction.

2. **Shared Modulus Attack**:
   - If two users share the same $N$, their private keys can be compromised:
     - $N_1 = pq, N_2 = pq'$
     - Compute $\gcd(N_1, N_2) = p$.

3. **Partial Key Leakage**:
   - Knowledge of $\phi(N)$ or $p+q$ enables factorization of $N$.

4. **Repeated Keys**:
   - If multiple users share the same modulus $N$, attackers can compute relationships between their private keys.

---

## Padding in RSA

**Purpose**: Mitigate structural vulnerabilities and ensure security.

1. **Signature Padding**:
   - PKCS#1 v1.5: $\text{0x01 || FF…FF || 00 || H(M)}$.
   - PKCS#1 v2.0: RSA-PSS with randomized padding for increased security.

2. **Encryption Padding**:
   - PKCS#1 v1.5: $\text{0x02 || Random Non-Zero Bytes || 00 || M}$.
   - RSA-OAEP (Optimal Asymmetric Encryption Padding): Adds randomness.

---

## Efficiency and Security

1. RSA keys are large and computationally inefficient compared to modern alternatives.
2. RSA is vulnerable to quantum attacks (e.g., Shor’s algorithm).
3. Modern alternatives:
   - **Elliptic Curve Cryptography (ECC)**: Smaller keys, more efficient, same security level.
   - **Post-Quantum Cryptography (PQC)**: Resistant to quantum computers.

---

## Conclusion

- RSA remains a foundational cryptographic algorithm but is increasingly replaced by ECC for efficiency and stronger security.
- Advances like RSA-OAEP and RSA-PSS address known vulnerabilities.
- Future cryptographic protocols will focus on PQC to counter quantum threats.

---
