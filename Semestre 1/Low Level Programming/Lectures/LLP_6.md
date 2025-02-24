
---

# Bit Hacks Deconstructed

## Key Concepts
- **Bit Hacks**: Techniques that use bitwise operations to perform calculations more efficiently, often without branching.
- **Premature Optimization**: These hacks are often premature optimizations, which are not always effective due to modern compilers and processors.

---

# Representation of Integers

## Unsigned Representation
- Example: 
  ```
  0000 0110 = 6
  ```

## Signed: Two’s Complement
- A negative value is represented as:
  - Flipping all bits of the positive value.
  - Adding 1 to the flipped value.
- Example: `-2` on 8 bits:
  ```
  1110 (1101 + 1)
  ```

---

# Bitwise Operations

## Bitwise AND, OR, NOT, XOR
- **AND (`&`)**: Both bits must be `1` for the result to be `1`.
  - Example: `0001 0011 & 0001 1000 = 0001 0000`
- **OR (`|`)**: Either bit must be `1` for the result to be `1`.
  - Example: `0001 0011 | 0001 1000 = 0001 1011`
- **NOT (`~`)**: Inverts all bits.
  - Example: `~0001 1000 = 1110 0111`
- **XOR (`^`)**: Result is `1` if the bits are different.
  - Example: `0001 0011 ^ 0001 1000 = 0000 1011`

## Bit Shifting
- **Left Shift (`<<`)**: Shifts bits to the left, introducing zeros.
  - Example: `0001 0011 << 3 = 1001 1000`
- **Right Shift (`>>`)**: Shifts bits to the right.
  - Example: `0001 0011 >> 3 = 0000 0010`

---

# Common Bit Hacks and Their Efficiency

## Compute the Sign of an Integer
- **Normal Code**: `sign = -(v < 0);`
- **Bit Hack**: `sign = v >> (sizeof(int) * CHAR_BIT - 1);`
- **Result**: Compilers generate similar code; no performance gain.

## Compute Absolute Value Without Branching
- **Objective**: Avoid branching, as branches can be expensive.
- **Normal Code**: `if (v < 0) r = -v; else r = v;`
- **Bit Hack**: 
  ```c
  int mask = v >> (sizeof(int) * CHAR_BIT - 1);
  r = (v + mask) ^ mask;
  ```
- **Result**: Same generated code by the compiler.

## Compute Minimum Without Branching
- **Normal Code**: `if (x < y) r = x; else r = y;`
- **Bit Hack**: `r = y ^ ((x ^ y) & -(x < y));`
- **Result**: Bit hack produces more complex code, no performance gain.

## Swap Two Integers
- **Normal Code**: Uses a temporary variable.
- **Bit Hack (XOR)**:
  ```c
  x ^= y; y ^= x; x ^= y;
  ```
- **Result**: No performance improvement.

## Determine If an Integer Is a Power of Two
- **Normal Code**:
  ```c
  for (int i = 1; i < sizeof(int) * CHAR_BIT; i++) {
    if (x == 1 << i) { f = 1; break; }
  }
  ```
- **Bit Hack**: `f = x && !(x & (x - 1));`
- **Result**: Bit hack is more concise and efficient compared to looping.

---

# Bit Hacks for Representing Sets

## Representation Using Bit Vectors
- Use an unsigned integer to represent a set, with each bit corresponding to an element.
- **Example**:
  ```
  Set containing integers: 4, 8, 9, 16, 17, 18, 23
  Representation: 0000 0000 1000 0111 0000 0011 0001 0000 (32 bits)
  ```

## Set Operations Using Bitwise Operations
- **Union (`|`)**: Combine elements from two sets.
- **Intersection (`&`)**: Find common elements between two sets.
- **Addition (`|`)**: Add an element to the set.
  - Example: `set |= 1 << n;`
- **Removal (`&` and `~`)**: Remove an element from the set.
  - Example: `set &= ~(1 << n);`
- **Flip Ownership (`^`)**: Toggle an element's presence.
  - Example: `set ^= 1 << n;`
- **Check if Element is in Set (`&`)**:
  ```c
  res = set & (1 << n);
  ```

## Population Count
- **Count the Number of Set Bits**:
  - **Normal Code**: Use a loop to count bits.
  - **Lookup Table**: Faster but involves memory operations.
  - **Divide and Conquer**: Efficient approach using successive bit grouping.

---

# Practical Applications

- **Cryptography**: Bit manipulation is crucial for encryption/decryption.
- **Device Drivers**: Manage hardware configurations using compact bit-level representation.
- **Network Protocols**: Protocol headers are represented in a compact form.
- **Board Games**: Used in problems like the n-queens puzzle.

## Example: Queens Problem
- Representing the board using three bit vectors (`down`, `right`, and `left`) to manage constraints while placing queens.

---

# Takeaway: Avoid Premature Optimization
- **Modern Compilers**: They are smart and optimize common bit hacks.
- **Readability**: Write clear, maintainable code. Let the compiler handle optimizations.
- **Bit Hacks**: Often fragile and non-portable. Useful in niche scenarios, but not always the best choice.

## Further Reading
- **Hacker's Delight**: [Link](https://doc.lagout.org/security/Hackers%20Delight.pdf)
- **Bit Twiddling Hacks by Eron Anderson**: [Link](https://graphics.stanford.edu/~seander/bithacks.html)

---
