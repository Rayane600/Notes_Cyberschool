# TP 2

## Write multi-precision multiplication of magnitude

The function that multiply two unsigned int and returns two unsigned int
(through pointers) is provided: 
```c
int uint_mul(UINT * rlow, UINT * rhigh, UINT a, UINT b);
```
A function to set to zero is provided:
```c
int mpint_set_zero(mpint_t a);
```

Now with the help of algorithm 14.12 from the Handobook of Applied Cryptography
[link to chapter 14 in PDF](https://cacr.uwaterloo.ca/hac/about/chap14.pdf)
write the multi-precision multiplication of magnitude of two mpint
```
int mpint_mul_mgn(mpint_t r, mpint_t a, mpint_t b);
```
Note that `mpint.h` contains
```c
#define ERROR_ABOVE_MPINTMAXSIZE 17
```
that can be returned if the length of the result is too large.

You will need also an auxiliary function to adjust `r->used`.
For example start from `MPINTMAXSIZE` and recreases while the high limbs of r are zero.
```c
void mpint_recompute_used(mpint_t r);
```

## Test your function

Generate the test-vector file by running the Python program
`generate_testvectors_mpint_mul_mgn.py`
It will generate a file `test_vector_mpint_mul_mgn.h`
Now consider `mpint_test_mul_mgn.c`

To compile, the instructions are
```bash
python3 generate_testvectors_mpint_mul_mgn.py # python3 or python

gcc -c mpint.c mpint_test_mul_mgn.c
gcc -o test_mpint_mul_mgn mpint.o mpint_test_mul_mgn.o
./test_mpint_mul_mgn
```

## Write multi-precision multiplication of mpint

re-use the previous function and consider the signs.
Fill-in the following array to help you decide the sign of the result:

| sign of a | sign of b | sign of r |
|:---------:|:---------:|:---------:|
| +         | +         |           |
| -         | +         |           |
| +         | -         |           |
| -         | -         |           |

## Test your function

```bash
python3 generate_testvectors_mpint_mul.py # only once

gcc -c mpint.c mpint_test_mul.c
gcc -o test_mpint_mul mpint.o mpint_test_mul.o
./test_mpint_mul
```

## if you are done:

### Write the single-precision multiplication with Karatsuba

```c
int uint_mul_karatsuba(UINT * rlow, UINT * rhigh, UINT a, UINT b);
```
(it should work but I did not write down the solution)
You should be able to re-use the test function and test file of `uint_mul`.

### Write multi-precision squaring

Note that the sign of a square is always positive.
(not tested).
You might need a single-precision squaring: you can re-use or adapt
`uint_mul`.

#### Write a test and test your function
For that, you can use `mpint_mul_mgn(r, a, a)` as the square is a particular case of multiplication.


### Write in/out functions

The functions given in TP 1 write the limbs of a, word-by-word.
With the next two functions we would like to write a mpint as a string, for
example to be able to compare or copy-paste in the Python command-line.

### print a mpint as a string in hexadecimal

```c
int mpint_print_str_hex(mpint_t a);
int mpint_println_str_hex(mpint_t a);
```

### converting a hexadecimal string to a mpint

Given a long string in hexadecimal digits, resp. decimal digits, initialize a mpint from that string.
(not tested).