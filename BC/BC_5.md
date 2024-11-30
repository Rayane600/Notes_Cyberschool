# Diffie-Hellman and Related Topics

---

## Prime Numbers

- A **prime number** $n \in \mathbb{Z}$ has only two divisors: $1$ and $n$.
  - **Example**: $7$ is prime; $9$ is not.

- **Sieve of Eratosthenes**: An algorithm to compute all primes up to a bound $B$.

- **Density of Primes**:
  - Let $\pi(B)$ be the number of primes less than or equal to $B$.
  - According to the **Prime Number Theorem**:
    $$
    \pi(B) \sim \frac{B}{\ln(B)}
    $$
  - The probability that a random integer less than $B$ is prime is approximately $\frac{1}{\ln(B)}$.

---

## Finite Fields: $\mathbb{Z}/p\mathbb{Z}$

- **Elements**: $\{0, 1, 2, \dots, p - 1\}$, where $p$ is a prime number.

- **Operations**: Addition, subtraction, and multiplication modulo $p$.

- **Modular Inverse**:
  - Every non-zero element $x$ has a modular inverse $y$ such that:
    $$
    xy \equiv 1 \pmod{p}
    $$

- **Multiplicative Group** $\mathbb{Z}/p\mathbb{Z}^*$:
  - Contains $\phi(p) = p - 1$ elements (since $p$ is prime, Euler's totient function $\phi(p) = p - 1$).

---

## Theorems

### Little Fermat's Theorem

For any integer $x$ not divisible by $p$:
$$
x^{p - 1} \equiv 1 \pmod{p}
$$

- **Example**: Compute $10^{123} \mod 7$:
  1. Note that $10 \mod 7 = 3$.
  2. Since $3^6 \equiv 1 \pmod{7}$ (by Little Fermat's Theorem):
     $$
     3^{123} = 3^{6 \times 20 + 3} \equiv (3^6)^{20} \cdot 3^3 \equiv 1^{20} \cdot 3^3 \equiv 27 \mod 7 \equiv 6
     $$
     So, $10^{123} \mod 7 = 6$.

### Euler's Theorem (Generalization)

If $N$ is a positive integer and $\gcd(x, N) = 1$, then:
$$
x^{\phi(N)} \equiv 1 \pmod{N}
$$
where $\phi(N)$ is Euler's totient function.

- **For $N = pq$** (where $p$ and $q$ are distinct primes):
  $$
  \phi(N) = (p - 1)(q - 1)
  $$

---

## Cyclic Groups

- In the multiplicative group $\mathbb{Z}/p\mathbb{Z}^*$, if $g$ is a **generator**, then:
  $$
  \text{Every non-zero element can be written as } g^k \mod p, \quad k \in \{0, 1, \dots, p - 2\}
  $$

- **Example**: For $p = 11$, let $g = 2$:
  - Compute the powers of $2 \mod 11$:
    $$
    1, 2, 4, 8, 5, 10, 9, 7, 3, 6, 1
    $$
  - Since all non-zero elements appear, $g = 2$ is a generator of $\mathbb{Z}/11\mathbb{Z}^*$.

- **Subgroups**:
  - Subgroups of $\mathbb{Z}/p\mathbb{Z}^*$ exist for every divisor $d$ of $p - 1$.
  - The order of a subgroup divides $p - 1$ (Lagrange's Theorem).

---

## Discrete Logarithm Problem (DLP)

- **Problem**:
  - Given $g$, $p$, and $y$, find $k$ such that:
    $$
    y \equiv g^k \mod p
    $$

- **Computational Hardness**:
  - For large prime-order subgroups, solving DLP is infeasible.
  - Algorithms like **Baby-Step Giant-Step** (time complexity $O(\sqrt{n})$) can solve DLP for small groups.

---

## Diffie-Hellman Key Exchange

- **Goal**: Establish a shared secret over an insecure channel.

1. **Public Parameters**: Prime $p$ and generator $g$.
2. **Alice**:
   - Chooses private key $a$, computes $A = g^a \mod p$, and sends $A$ to Bob.
3. **Bob**:
   - Chooses private key $b$, computes $B = g^b \mod p$, and sends $B$ to Alice.
4. **Shared Secret**:
   - Alice computes $K = B^a \mod p$.
   - Bob computes $K = A^b \mod p$.
   - Both derive the same $K = g^{ab} \mod p$.

---

## ElGamal Encryption

- **Public Key**: $B = g^b \mod p$.
- **Private Key**: $b$.

1. **Encryption**:
   - Alice chooses random $a$, computes $u = g^a \mod p$, $s = B^a \mod p$.
   - Encrypts message $m$ as $c = (u, v)$, where $v = E_s(m)$.

2. **Decryption**:
   - Bob computes $s = u^b \mod p$ and decrypts $m = D_s(v)$.

---

## Zero-Knowledge Proof (ZKP)

- **Goal**: Prove knowledge of a secret without revealing it.

1. Bob computes $t = g^k \mod p$ for random $k$, sends $t$.
2. Verifier sends challenge $c$.
3. Bob responds with $z = k + cb \mod p-1$.
4. Verifier checks $g^z \equiv t \cdot B^c \mod p$.

---

## Schnorr Signature Scheme

1. **Signing**:
   - Compute $t = g^k \mod p$, $c = H(B, m, t)$, and $z = k + cb \mod p-1$.
   - Signature: $(c, z)$.

2. **Verification**:
   - Compute $t' = g^z \cdot B^{-c} \mod p$, verify $c' = c$.

---

## TLS and Signal Protocols

- **TLS**: Uses Diffie-Hellman for secure key exchange.
- **Signal**: Builds on Diffie-Hellman with forward secrecy and post-compromise security.

---
