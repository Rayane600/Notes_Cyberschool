# TP 3: Modular arithmetic

In this setting, the modular intergers are represented with integers from 0 to 
_m_ - 1.
To simplify, we will continue to use the structure `mpint_t`.
By default, a modular integer has positive sign `ZPOS`.

## write modular addition
The modular addition of two modular integers is 
_r_ = _a_ + _b_ mod _m_ where _m_ is the module.
If the result of _a_ + _b_ over the integers is greater than _m_ then subtract _m_ to this result.

```c
int modint_add(mpint_t r, mpint_t m, mpint_t a, mpint_t b)
```

## write modular subtraction
The modular subtraction of two modular integers is 
_r_ = _a_ - _b_ mod _m_ where _m_ is the module.
If the result of _a_ - _b_ is negative then add _m_ to this result.

```c
int modint_sub(mpint_t r, mpint_t m, mpint_t a, mpint_t b)
```

## write modular negation
```c
int modint_neg(mpint_t r, mpint_t m, mpint_t a)
```
Result _r_ = - _a_ mod _m_

## multiplication by a power of 2 of the word size

This is much faster than a multiplication by a simple-precision word.
Multiply a `mpint_t` _a_ by `2^WORD` (that is, `2^32` or `2^64`).
It means shift the words in `a->limbs`.

## simple precision to multi-precision multiplication
Multiplication of a multi-precision integer _a_ by a single-precision word _u_

```c
int mpint_uint_mul_mgn(mpint_t r, UINT u, mpint_t a)
```
The function `uint_mul` can be used.
