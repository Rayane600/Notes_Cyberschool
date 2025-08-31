
32 bit

| 32-bit | 16-bit | 8-bit | save convention | Typical purpose                  |
| ------ | ------ | ----- | --------------- | -------------------------------- |
| EAX    | AX     | AH/AL | caller          | Accumulator, return value        |
| EBX    | BX     | BH/BL | callee          | PIC base / data pointer          |
| ECX    | CX     | CH/CL | caller          | Loop & shift counter             |
| EDX    | DX     | DH/DL | caller          | High part of MUL/DIV, I/O port   |
| ESI    | SI     | —     | callee          | Source index for string ops      |
| EDI    | DI     | —     | callee          | Destination index for string ops |
| EBP    | BP     | —     | callee          | Frame pointer (optional)         |
| ESP    | SP     | —     | —               | Stack pointer                    |
| EIP    | IP     | —     | —               | Next instruction                 |
64 bit

| 64-bit | 32-bit | save convention | Function-call role / comment   |
| ------ | ------ | --------------- | ------------------------------ |
| RDI    | EDI    | caller          | 1st  argument                  |
| RSI    | ESI    | caller          | 2nd argument                   |
| RDX    | EDX    | caller          | 3rd argument                   |
| RCX    | ECX    | caller          | 4th argument                   |
| R8     | R8D    | caller          | 5th argument                   |
| R9     | R9D    | caller          | 6th argument                   |
| RAX    | EAX    | caller          | Return value, syscall #        |
| R10    | R10D   | caller          | volatile/temp, 4th syscall arg |
| R11    | R11D   | caller          | volatile/temp                  |
| RBX    | EBX    | callee          | Callee-saved, general purpose  |
| RBP    | EBP    | callee          | Optional frame pointer         |
| R12    | R12D   | callee          | general purpose                |
| R13    | R13D   | callee          | general purpose                |
| R14    | R14D   | callee          | general purpose                |
| R15    | R15D   | callee          | general purpose                |
| RSP    | ESP    | —               | Stack pointer                  |
| RIP    | EIP*   | —               | Next instruction               |
|        |        |                 |                                |
Instructions :

| Group                   | AT&T Instruction             | C Analogue                                            |
|------------------------|------------------------------|--------------------------------------------------------|
| **Data move / address**| `movl msg(%rip), %eax`       | `eax = msg;`                                          |
|                        | `leaq (%rdi,%rcx,4), %rax`   | `rax = &rdi[rcx];` (compute address only)             |
| **Stack**              | `pushq %rbx`                 | `*(--rsp) = rbx;` (save)                              |
|                        | `popq %rbx`                  | `rbx = *rsp++;` (restore)                             |
| **Arithmetic**         | `addl $10, %eax`             | `eax += 10;`                                          |
|                        | `incl %ecx`                  | `++ecx;`                                              |
|                        | `imull %ebx, %eax`           | `eax *= ebx;` (signed multiplication)                 |
| **Logic / bit**        | `xorl %eax, %eax`            | `eax = 0;` (fast clear)                               |
|                        | `sarl $1, %edx`              | `edx >>= 1;` (signed right-shift)                     |
|                        | `bsfl %edx, %eax`            | `eax = first_set_bit(edx);`                           |
| **Compare / test**     | `cmpl %ebx, %eax`            | Sets flags as if `eax - ebx`; next `jcc` may branch   |
|                        | `testq %rdi, %rdi`           | Sets flags as if `rdi & rdi`; idiomatic `if (rdi==0)` |
| **Branches**           | `je done`                    | `if (ZF) goto done;`                                  |
|                        | `jmp loop`                   | Unconditional `goto`                                  |
| **Calls / returns**    | `call printf`                | Push return addr, jump → `printf();`                  |
|                        | `ret $8`                     | `return;` (and pop `n` arg bytes in `stdcall`)        |
| **Syscalls**           | `movl $1, %eax; int $0x80`   | `exit(1);` → into kernel                              |
|                        | `movq $60, %rax; syscall`    | `exit(0);` (args in `%rdi`, `%rsi`, ...)              |
