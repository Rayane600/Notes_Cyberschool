# x86 Assembly 

## General Overview

### Registers
- **SP**: Stack Pointer (top of the stack)
- **PC**: Program Counter (address of next instruction)
- **GPR**: General Purpose Registers (temporary data storage)

### Address-space Layout
- **Stack**: Local/automatic variables
- **Heap**: Dynamic allocation (malloc, new)
- **Data (BSS)**: Static uninitialized variables (zero-initialized)
- **Data**: Static initialized variables
- **R/O Data**: Constant values (read-only)
- **Text**: Executable code

---

## x86-32 Architecture

### General Purpose Registers (GPR)
| Register | Purpose                                     |
|----------|---------------------------------------------|
| **EAX**  | Accumulator (math results, return values)   |
| **EBX**  | Base Address (data structure pointers)      |
| **ECX**  | Counter (loop counters)                     |
| **EDX**  | Data Register (multiplication/division ops) |

*Example*:
```assembly
mov eax, 1    ; move 1 into EAX
add eax, ebx  ; add EBX to EAX
```

### Index and Pointer Registers
| Register | Purpose                                   |
|----------|-------------------------------------------|
| **ESP**  | Stack Pointer (top of stack)              |
| **EBP**  | Base Pointer (base of stack frame)        |
| **ESI**  | Source Index (source for data transfers)  |
| **EDI**  | Destination Index (destination for data transfers) |
| **EIP**  | Instruction Pointer (next instruction)    |

### Segment Registers
- **CS**: Code Segment
- **DS**: Data Segment
- **SS**: Stack Segment
- **ES, FS, GS**: Extra Data Segments (e.g., video memory)

### EFLAGS Register (important bits)
- **CF**: Carry Flag (arithmetic overflow)
- **ZF**: Zero Flag (result is zero)
- **SF**: Sign Flag (negative result)
- **OF**: Overflow Flag (arithmetic overflow)

---

## Stack Operations

Stack works on a **LIFO** basis.

### Basic Instructions
| Instruction | Description                     |
|-------------|---------------------------------|
| `push src`  | Pushes value onto the stack     |
| `pop dst`   | Pops value from the stack       |

*Example*:
```assembly
push eax  ; Pushes the value of EAX onto stack
pop ebx   ; Pops top stack value into EBX
```

### Stack Frame Management
Each function call creates a stack frame, identified by `(ESP, EBP)`.

- **ESP**: Stack pointer (top element, lowest address)
- **EBP**: Base pointer (base of current stack frame, highest address)

### Stack Frame Instructions
| Instruction | Operation                                     |
|-------------|-----------------------------------------------|
| `enter`     | Saves previous EBP, sets up new stack frame   |
| `leave`     | Restores previous stack frame                 |

*Example*:
```assembly
enter 0, 0  ; setup stack frame
; your code here
leave       ; cleanup stack frame
```

---

## Call Stack and Functions

### Managing Function Calls
- On function call, the next instruction pointer (**EIP**) is saved onto the stack.
- Upon return (`ret`), **EIP** is popped off stack to resume execution.

### Instructions
| Instruction | Description                           |
|-------------|---------------------------------------|
| `call addr` | Save current EIP, jump to function    |
| `ret`       | Restore saved EIP, return execution   |

*Example*:
```assembly
call myFunction

myFunction:
  push ebp
  mov ebp, esp
  ; function body
  pop ebp
  ret
```
