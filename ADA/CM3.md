
---

# ADA 3: Computational Linear Algebra

## Key Concepts
- **Linear Algebra**: Deals with matrices, vector spaces, linear maps, and their operations.
- **Agenda**: Recall matrix properties, multiplication, Gaussian elimination, matrix factorization, and applications to graphs.

---

# Vector Space and Linear Maps

## Vector Space
- **Definition**: $\mathbb{R}^n$ is the set of $n$-dimensional vectors with coordinates over $\mathbb{R}$.
- **Properties**:
  - If $v \in \mathbb{R}^n$ and $\lambda \in \mathbb{R}$, then $\lambda v \in \mathbb{R}^n$.
  - If $u, v \in \mathbb{R}^n$, then $u + v \in \mathbb{R}^n$.
  - **Linear Combination**: $\sum_{i=1}^{k} \alpha_i u_i$, where $\alpha_i$ are real numbers.
- **Basis**: A set of vectors that is both generating and independent. Any vector can be uniquely represented by a combination of the basis vectors.

## Matrix as a Subspace
- **Example**: $u = (1, 0, 0)$ and $v = (0, 1, 0)$ generate a subspace of $\mathbb{R}^3$ (the plane $z = 0$).
- **Rank**: Represents the number of independent vectors. Full rank means that the matrix spans the entire space.

---

# Matrix Representation and Operations

## Linear Maps as Matrices
- A matrix $A \in \mathbb{R}^{m \times n}$ represents a linear map from $\mathbb{R}^n$ to $\mathbb{R}^m$.
- **Composition**: If matrices $A$ and $B$ represent linear maps $f$ and $g$, then the composition $f \circ g$ is represented by $AB$.
- **Identity and Inversion**:
  - The identity matrix represents $f(x) = x$.
  - A matrix $A$ with a non-zero determinant can be inverted to obtain $A^{-1}$, representing the inverse map.

## Important Examples
- **Polynomials**: The set of polynomials of degree $< n$, denoted as $\mathbb{R}^n[X]$, forms a vector space.
  - Basis: $(1, X, X^2, \ldots, X^{n-1})$.
  - **Lagrange Polynomials** represent an alternate basis.

---

# Gaussian Elimination

## Definition
- **Gaussian Elimination**: A systematic way to solve linear systems, find matrix inverses, and determine ranks.
- **Augmented Matrix**: $[A | b]$ used to solve $Ax = b$.
- **Complexity**: $O(n^3)$ for an $n \times n$ matrix.

## Procedure
1. Transform matrix $A$ into the identity matrix by performing elementary row operations.
2. Apply the same operations on the identity matrix to find $A^{-1}$.
3. Solve $Ax = b$ by transforming $A$ and $b$.

## Kernels and Images
- **Image**: The set of vectors that can be reached by applying a linear transformation represented by matrix $A$. It is noted as $\text{Im}(A)$.
- **Kernel**: The set of vectors that map to zero under transformation $A$. Noted as $\text{Ker}(A)$.
- **Rank-Nullity Theorem**: $\text{dim}(\text{Im}(A)) + \text{dim}(\text{Ker}(A)) = n$.

---

# Matrix Factorizations

## LU Decomposition
- **Definition**: $A = LU$, where $L$ is lower triangular, and $U$ is upper triangular.
- **Application**: Solve $Ax = b$ by solving $LUx = b$.

## QR Factorization
- **Definition**: $A = QR$, where $Q$ is orthogonal, and $R$ is upper triangular.
- **Stability**: QR is more numerically stable than Gaussian elimination.
- **Orthogonal Vectors**: Found using the Gram-Schmidt orthogonalization process.

## Diagonalization
- **Definition**: $A = PDP^{-1}$, where $P$ is the set of eigenvectors, and $D$ is a diagonal matrix of eigenvalues.
- **Conditions**: Not all matrices are diagonalizable, but triangularization is always possible.

---

# Matrix Multiplication and Optimization

## Classical Multiplication
- Compute $AB$ by evaluating each column of $B$ using $A$.
- **Strassen's Algorithm**:
  - Divide-and-conquer approach.
  - Complexity: $$ T(n) = 7T(n/2) + O(n^2) $$

---

# Applications in Graph Theory

## Adjacency Matrices
- **Definition**: Represents a graph with $n$ vertices, where $a_{ij}$ indicates an edge between vertices $i$ and $j$.
- **Shortest Paths**:
  - Compute $A^2$ with modified operations to determine shortest paths of length at most 2.
  - Compute $A^n$ to determine all shortest paths in the graph.
  - Complexity: Matrix multiplication cost is $O(n^3)$.

---

# Evaluation and Interpolation

## Van der Monde Matrix
- **Interpolation Problem**: Represented as a linear system using a Van der Monde matrix $V$.
- **Invertibility**: $V$ is invertible if evaluation points are distinct.
- **Complexity**: Evaluation and interpolation can be performed in $O(n^2)$ if $V$ and $V^{-1}$ are precomputed.

## Fourier Transform
- **Circular Matrices**: Can be diagonalized as $VDV^{-1}$.
- **Representation**: Can be viewed as a polynomial, and diagonal elements are evaluations by the $n$-th root of unity.

---

# Conclusion

- **Computational Linear Algebra**: The foundation of efficiently solving problems in linear algebra.
- **Gaussian Elimination**: Complexity of $O(n^3)$.
- **Stability**: Numerical issues in matrix inversion can be solved using more stable algorithms like QR factorization.
- **Applications**: Linear algebra allows solving linear systems, finding images, computing kernels, and working with graph representations.

---