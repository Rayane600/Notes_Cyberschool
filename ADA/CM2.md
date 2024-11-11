w
---

# ADA 2: Divide-and-Conquer in Numerical Analysis

## Key Concepts
- **Divide-and-Conquer Algorithms**: Techniques that recursively break down a problem into smaller subproblems, solve each independently, and combine their results.
- **Applications**: Numerical algorithms like GCD, multiplication, FFT.

---

# Asymptotic Notations

## Big-O Notation
- **Definition**: $T(n) = O(f(n))$ iff there exist positive constants $c$ and $n_0$ such that $T(n) \leq c \cdot f(n)$ for all $n \geq n_0$.
- **Example**: $T(n) = 12n^2 + 5n + 100$ is $O(n^2)$.

## Little-o Notation
- **Definition**: $T(n) = o(f(n))$ iff for every positive constant $c > 0$, there exists an $n_0$ such that $T(n) \leq c \cdot f(n)$ for all $n \geq n_0$.
- **Mathematical Condition**: $\lim_{n \to \infty} \frac{T(n)}{f(n)} = 0$.

## Common Growth Rates
- $\log(\log n) \ll \log n \ll \sqrt{n} \ll n \ll n^2 \ll n^2 \log n \ll n^3 \ll e^n$

---

# Divide-and-Conquer Algorithms

## General Recurrence Relation
- **Assumption**: Input of size $n$ is split into subproblems of size $n/b$.
- **Complexity Formula**: $$T(n) = aT(n/b) + O(n^d)$$ where $a \geq 1$, $b > 1$, and $d \geq 0$.

### Master Theorem Cases
- **Case 1**: $T(n) = O(n^d \log n)$ if $a = b^d$
- **Case 2**: $T(n) = O(n^d)$ if $a < b^d$
- **Case 3**: $T(n) = O(n^{\log_b(a)})$ if $a > b^d$

### Example
- **Merge Sort**: 
  - Recurrence relation: $T(n) = 2T(n/2) + O(n)$
  - Complexity: $T(n) = O(n \log n)$

## Numerical Algorithm Examples
- **Binary Search**: Reduces problem size by half each iteration.
  - Complexity: $O(\log n)$

- **Karatsuba Multiplication**:
  - Faster multiplication of large integers compared to naive $O(n^2)$ multiplication.
  - Recurrence: $T(n) = 3T(n/2) + O(n)$
  - Complexity: $O(n^{\log_2(3)})$

---

# Euclidean Algorithm (GCD)

## Iterative Version
- **Algorithm**: 
  ```python
  def gcd(a, b):
      while a > b:
          a, b = b, a % b
      return a
  ```
- **Analysis**: In each iteration, the remainder is reduced, requiring $O(\log n)$ iterations in the worst case.
- **Complexity**: $O(|a||b|)$

---

# Fast Modular Exponentiation

## Recursive Version
- **Algorithm**: Computes $a^e \mod n$ efficiently using divide-and-conquer.
  ```python
  def expmod(a, e, n):
      if e % 2 == 0:
          return expmod((a * a) % n, e // 2, n)
      else:
          return (a * expmod((a * a) % n, e // 2, n)) % n
  ```
- **Complexity**: $O(|e| \cdot |n|^2)$ in the worst case.

---

# Polynomial Evaluation and Interpolation

## Horner's Scheme
- **Polynomial Representation**: $A(X) = \sum_{i=0}^{n-1} a_i X^i$
- **Horner's Scheme**: Efficient method to evaluate polynomials.
  - Complexity: $O(n)$ for evaluation.

## FFT (Fast Fourier Transform)
- **Usage**: Used for polynomial multiplication and signal processing.
- **Complexity**: $O(n \log n)$
- **Inverse FFT**: Used for polynomial interpolation with the same complexity.

---

# Toom-Cook and Fast Multiplication

## Toom-Cook Algorithm
- **Generalization of Karatsuba** for splitting integers into more than two parts.
- **Example**: Toom-3 splits into three parts.
  - Complexity: $$T(n) = 5T(n/3) + O(n)$$
  - Complexity: $O(n^{\log_3(5)})$

## Fast Fourier Transform (FFT) and Schonhage-Strassen
- **FFT**: $O(n \log n)$ multiplication.
- **Schonhage-Strassen**: Used for very large integer multiplication.
  - Complexity: $O(n \log n \log \log n)$

---

# Newton's Method for Finding Roots

## Algorithm
- **Goal**: Find a zero of a function $f(x) = 0$.
- **Iteration Formula**: $$x_{i+1} = x_i - \frac{f(x_i)}{f'(x_i)}$$
- **Example**: Finding $\sqrt{2}$ as a zero of $f(x) = x^2 - 2$.

---

# Summary

- **Divide-and-Conquer**: Useful for breaking problems into manageable subproblems.
- **Master Theorem**: Essential tool for analyzing recursive divide-and-conquer algorithms.
- **Fast Algorithms**:
  - **Karatsuba** and **Toom-Cook** for fast multiplication.
  - **FFT** for polynomial evaluation and signal processing.
- **Euclidean Algorithm**: Efficient for computing GCD.
- **Newton's Method**: Used for root finding and other optimizations.

---