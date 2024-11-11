

---

# Assembly Languages

## Pros and Cons
- **Pros**:
  - Understand machine-level operations.
  - Debugging and reverse engineering.
  - Hardware-dependent optimizations (e.g., drivers).
  
- **Cons**:
  - Lack of portability across devices and processors.
  - Difficult to read, understand, and optimize.

## Key Terminology
- **Variables**: Restricted to processor registers and memory locations.
- **Instruction Set**: Set of commands supported by the CPU.
- **Mnemonic**: Human-readable name of an instruction (e.g., `movl`).
- **Operand(s)**: Arguments of an instruction.

### Example
```asm
movl $7, 80(%rip)  ; Opcode: c7, Mnemonic: movl, Operand: 7 and 80(%rip)
```

---

# Intel x86 Family

## Architectures
- **x86-32 Names**: AMD (x86), Intel (IA-32), GCC (i386).
- **x86-64 Names**: AMD (x86-64, AMD64), Intel (Intel 64), GCC (amd64).

## Registers
- **x86-32 Registers**:
  - **Data Registers**: EAX, EBX, ECX, EDX.
  - **Index & Pointer Registers**: EBP (Base Pointer), ESP (Stack Pointer), ESI, EDI, EIP.
  - **Segment Registers**: CS, DS, SS, etc.
  - **Flag Register**: EFLAGS.
  - **Floating Point Registers**: ST0-ST7.

- **x86-64 Registers**: Longer versions of existing x86-32 registers (e.g., RAX-RDX), plus new ones (R8-R15).

---

# Process Memory Layout

## Memory Regions
- **Code** (`.text`)
- **Data** (`.data`, `.bss`)
- **Heap**: Dynamically allocated memory.
- **Stack**: Stores local data for functions.

### Example Memory Layout
```
0x00000000
  [Code]
  [Data]
  [Heap]
  [Stack]
0xffffffff
```

## Registers for Memory Management
- **Program Counter (PC)**: Points to the next instruction.
- **Stack Pointer (SP)**: Points to the current top of the stack.
- **General Purpose Registers (GPR)**: For general usage.

---

# Instructions Set

## Data Movement Instructions
- **Memory Instructions**:
  - `mov`: Moves data from source to destination.
  - `xchg`: Exchanges the contents of two operands.
  - `lea`: Loads the effective address.

### Example
```asm
movl $8, %ebx  ; %ebx = 8
xchg %eax, %ebx  ; Exchange contents of %eax and %ebx
lea 2(%esp), %ecx  ; Load address esp+2 into %ecx
```

## Arithmetic & Logical Instructions
- **Addition/Subtraction**: `add`, `sub`
- **Multiplication/Division**: `mul`, `div`
- **Bitwise Operations**: `and`, `or`, `xor`, `not`
- **Shifts**: `shl`, `shr`, `sar`

### Example
```asm
addl $30, %eax  ; %eax = %eax + 30
subl $7, %eax  ; %eax = %eax - 7
```

## Control Flow Instructions
- **Jumps** (`jmp`, `je`, `jne`) based on condition flags (e.g., Zero Flag `ZF`, Carry Flag `CF`).

---

# Stack Management

## Stack Instructions
- **Push**: Push content onto the stack.
- **Pop**: Pop content from the stack.

### Example
```asm
push %eax  ; Pushes %eax value onto the stack
pop %ebx  ; Pops the top of the stack into %ebx
```

## Stack Frame Management
- **Enter/Leave** Instructions:
  - **Enter** creates a new stack frame.
  - **Leave** restores the previous stack frame.

---

# Application Binary Interface (ABI)

## SystemV i386 ABI
- **Calling Conventions**: Defines how functions receive parameters and return values.
  - **Volatile Registers**: Can be overwritten (e.g., `eax`, `ecx`).
  - **Non-volatile Registers**: Must be preserved (e.g., `ebx`).

### Function Calling Steps
1. Push parameters onto the stack (caller).
2. Execute the `call` instruction.
3. Allocate stack frame (callee).
4. Preserve registers if needed (callee).
5. Execute function code.
6. Deallocate stack frame (callee).
7. Execute `ret` to return (callee).

### Example Assembly Code for Function Call
```asm
pushl %ebp       ; Save base pointer
movl %esp, %ebp  ; New base pointer
movl 12(%ebp), %eax  ; Load argument from stack
cmp %eax, 8(%ebp)  ; Compare arguments
```

## x86-64 vs x86-32 Differences
- Mostly similar instructions.
- **x86-64** provides additional registers (`R8-R15`).
- Calling conventions and register usage may differ due to extended register set.

---
