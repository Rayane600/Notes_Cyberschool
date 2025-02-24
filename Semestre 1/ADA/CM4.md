
---
# ADA4: Randomized Algorithms and Prime Number Generation

**Probability**

1. **Definitions**
    
    - Probability distribution $P: U \to [0,1]$ satisfies $\sum_{x \in U} P(x) = 1$.
        - Examples:
            - Uniform distribution: $P(x) = \frac{1}{|U|}$.
            - Point distribution at $x_0$: $P(x_0) = 1$, $P(x) = 0$ for $x \neq x_0$.
    - Entropy: $H(P) := -\sum_{x \in U} P(x) \log(P(x))$. Maximized for uniform distribution: $H(P) = \log(|U|)$.
2. **Events**
    
    - Probability of an event $A \subseteq U$: $\text{Pr}[A] = \sum_{x \in A} P(x)$.
    - Union Bound:
        - $\text{Pr}[A_1 \cup A_2] \leq \text{Pr}[A_1] + \text{Pr}[A_2]$.
        - $\text{Pr}[A_1 \cup A_2] = \text{Pr}[A_1] + \text{Pr}[A_2] - \text{Pr}[A_1 \cap A_2]$.
3. **Random Variables**
    
    - Definition: $X: U \to V$ induces a distribution $\text{Pr}[X = v] := \text{Pr}[X^{-1}(v)]$.
    - Uniform Random Variables: $r \leftarrow_R U$ denotes $r$ is uniformly random over $U$, with $\text{Pr}[r = a] = \frac{1}{|U|}$ for all $a \in U$.

---

**Randomized Algorithms**

1. Deterministic Algorithm: $y \leftarrow A(m)$.
2. Randomized Algorithm: $y \leftarrow_R A(m; r)$ where $r \leftarrow_R {0,1}^n$.

Examples:

- **Las Vegas Algorithm**: Always correct; runtime depends on input (e.g., randomized quicksort).
- **Monte Carlo Algorithm**: Output may be incorrect with a small probability.

3. **Complexity Classes**
    - RP (Randomized Polynomial Time):
        - If $x \in L$, $\text{Pr}[A(x) \text{ accepts}] \geq \frac{1}{2}$.
        - If $x \not\in L$, $\text{Pr}[A(x) \text{ accepts}] = 0$.
    - BPP (Bounded Polynomial Time):
        - If $x \in L$, $\text{Pr}[A(x) \text{ accepts}] \geq \frac{3}{4}$.
        - If $x \not\in L$, $\text{Pr}[A(x) \text{ accepts}] \leq \frac{1}{4}$.

---

**Prime Number Generation**

1. **Definitions**
    
    - Prime Number: Integer $n$ with divisors ${1, n}$.
    - Composite Number: Integer with more than two divisors.
    - Coprime Numbers: $\text{gcd}(a, b) = 1$.
2. **Algorithms**
    
    - **Eratosthenes Sieving**
        
        - Complexity: $O(n \log n \log \log n)$.
        - Removes multiples of primes $\leq \sqrt{n}$.
    - **Primality Tests**
        
        - Fermat Primality Test:
            - If $a^{n-1} \equiv 1 \mod n$, then $n$ may be prime.
            - Fails for Carmichael numbers, which satisfy $a^{n-1} \equiv 1 \mod n$ for all $a$ but are composite.
        - Miller-Rabin Test:
            - Let $n-1 = 2^s t$ with $t$ odd.
            - If $a^{n-1} \not\equiv 1 \mod n$ for some $a$, $n$ is composite.
            - Probability of error $< \frac{1}{2^m}$ after $m$ trials.
    - AKS Algorithm:
        
        - Deterministic primality test (2001).
        - Complexity: $O(\log(n)^6)$, slower than Miller-Rabin.

---

**Number Theory**

1. Modular Arithmetic
    
    - $(\mathbb{Z}/n\mathbb{Z})$: Residue classes modulo $n$.
    - $(\mathbb{Z}/n\mathbb{Z})^*$: Coprime residue classes modulo $n$.
2. Extended Euclidean Algorithm (EEA)
    
    - Computes $\text{gcd}(a, b)$ and coefficients $u, v$ such that $au + bv = \text{gcd}(a, b)$.
3. Euler’s Theorem
    - $a^{\phi(n)} \equiv 1 \mod n$, where $\phi(n)$ is Euler's totient function.
    - Generalizes Fermat's Little Theorem.

---

**Prime Number Theorems**

1. Prime Number Theorem: $\pi(N) \sim \frac{N}{\ln(N)}$, where $\pi(N)$ is the number of primes $\leq N$.
2. Density: The probability a random integer $\leq N$ is prime is $\frac{1}{\ln(N)}$.

---

**Advanced Topics**

1. Chinese Remainder Theorem:
    
    - Compact representation of residues modulo $p$ and $q$ if $N = pq$.
2. Quadratic Residues:
    
    - $y$ is a square modulo $N$ if $\exists x$ such that $x^2 \equiv y \mod N$.
3. Pocklington Criterion:
    
    - Partial factorization of $N-1$ can prove primality of $N$.

---
