# TP5

Ce TP sera noté.
A rendre pour le lundi 18 novembre 9:45.

## Exponentiation modulaire

Programmer l'exponentiation modulaire avec l'algorithme square-and-multiply vu au cours 4.
```c
int modint_exp_mpint(mpint_t r, mpint_t a, mpint_t e, mpint_t m, UINT m_prime){
	/* r = a^e modulo m using Montgomery multiplication
	 * [in] a: 1st operand, modular multi-precision integer, assumes it is reduced: 0 <= a < m
	 * [in] e: exponent, positive multi-precision integer
	 * [in] m: module, positive multi-precision integer
	 * [in] m_prime: single-precision positive integer, inverse of m modulo 2^WORD
	 * [out] r: result, modular multi-precision integer, reduced: 0 <= r < m
     */
```

Il y aura besoin de réutiliser la fonction de multiplication modulaire dont l'en-tête dans `mpint.h` est
```c
int modint_mul(mpint_t r, mpint_t a, mpint_t b, mpint_t m, UINT _prime);
```

Pour lire les bits de gauche à droite, c'est-à-dire du plus significatif au moins significatif :
```c
i = WORD-1; /* bit le plus significatif */
bi = (ei >> i) & 1; /* bit i */
```