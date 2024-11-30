
# ADA 5: Factorization and Discrete Logarithm

---

### **Factorization**

**Key Idea**:

- Factorization involves finding non-trivial divisors (other than $1$ and $n$) of an integer $n$.
- The problem becomes hard when $n = pq$, where $p$ and $q$ are large primes of similar size.

---

**Naive Factorization**:

- Try dividing $n$ by all primes $\leq \sqrt{n}$.
- Complexity: $O(\sqrt{n}/\log(\sqrt{n})) \approx O(n^{1/2}/\log(n))$.
- Input size $N = \log n$ bits $\implies$ Exponential complexity: $2^{N/2}/\log(\log N)$.

---

**Fermat Factorization**:

- Assumes $n$ is the difference of two squares: $n = a^2 - b^2 = (a-b)(a+b)$.
    
- Algorithm:
    1. Start with $a = \lceil \sqrt{n} \rceil$.
    2. If $a^2 - n$ is a perfect square $b^2$, then $n = (a-b)(a+b)$.
    3. If not, increment $a$ and repeat.
- Complexity: Efficient if $p, q$ (factors of $n$) are close. If they are far apart, the process becomes slow.
    

---

**Quadratic Sieve**:

- Extends Fermat’s idea using congruences: $a^2 \equiv b^2 \mod n$.
- Finds smooth numbers (numbers whose factors are $\leq B$).
- Complexity: Subexponential $O(\exp(c \sqrt{\ln(n) \ln(\ln(n))}))$.
- Faster than Fermat for large $n$.

---

**(P-1) Method**:

- If $n$ has a factor $p$ such that $p-1$ is $B$-smooth:
    1. Compute $B!$.
    2. Use Fermat's Little Theorem: $a^{B!} \equiv 1 \mod p$.
    3. Compute $\gcd(a^{B!} - 1, n) = p$.
- Efficiency depends on the size of $B$.

---

**Pollard’s Rho Algorithm**:

- Detects cycles using a random function $f(x) = x^2 + 1 \mod n$.
    
- Efficient for smaller factors:
    
    1. Start with $x_0$.
    2. Compute a sequence $x_{i+1} = f(x_i)$.
    3. Detect collisions using Floyd’s cycle detection.
- Complexity: $O(\sqrt{p})$ for $p$ (smallest factor of $n$).
    

---

**Strassen’s Algorithm**:

- Computes products and evaluations in polynomial rings modulo $n$.
- Product tree approach divides problems into subproblems.
- Complexity: $O(M(k) \log k)$, where $M(k)$ is the cost of multiplying degree-$k$ polynomials.

---

**Summary of Factorization Complexity**:

- Naive: $O(n^{1/2})$.
- Fermat: Efficient if factors are close.
- Quadratic Sieve: Subexponential.
- (P-1): Efficient for $B$-smooth primes.
- Pollard’s Rho: $O(\sqrt{p})$.
- Strassen: $O(M(k) \log k)$.

---

### **Discrete Logarithm Problem (DLP)**

**Definition**:

- Given $g, p$, and $y$, find $x$ such that $y = g^x \mod p$.
- $(\mathbb{Z}/p\mathbb{Z})^*$ is a cyclic group with generator $g$.

---

**Baby Step-Giant Step Algorithm**:

- Assume $p$ is a prime and $y = g^x \mod p$.
    
- Divide $x$ into two parts: $x = am + b$ where $m = \lceil \sqrt{p} \rceil$.
    
    1. Compute a table of $g^{im} \mod p$ for $i = 0, 1, \ldots, m$.
    2. For each $j$, find $h g^{-j} \mod p$ in the table.
    3. Collision reveals $a$ and $b$.
- Complexity: $O(\sqrt{p})$.
    

---

**Pohlig-Hellman Algorithm**:

- Reduces DLP to subgroups of $(\mathbb{Z}/p\mathbb{Z})^*$.
    
- Assumes $p-1 = p_1^{e_1} p_2^{e_2} \ldots p_k^{e_k}$.
    
- Uses the Chinese Remainder Theorem to reconstruct $x \mod p-1$.
    
- Complexity: $O(\sqrt{p_{\text{max}}})$, where $p_{\text{max}}$ is the largest prime factor of $p-1$.
    

---

**Shor's Algorithm (Quantum)**:

- Uses periodicity of functions to solve DLP and factorization problems in polynomial time $O((\log n)^2)$.

---