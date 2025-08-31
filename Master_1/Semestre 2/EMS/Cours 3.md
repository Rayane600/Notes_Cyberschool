# ARM Cortex-M3 

---

## Processor Basics
- **CPU Function**: Fetches, decodes, and executes instructions stored in memory.
- **Components**: ALU, registers, control logic, memory buses.
- **ISA vs. µArch**:
  - **ISA**: Instruction Set Architecture (e.g., ARM, x86). Defines instructions and their behavior.
  - **µArch**: Implementation details (pipeline depth, caches, branch prediction).

---

## ARM Overview
- **Business Model**: Licenses CPU designs (RISC-based) to companies like STM, Qualcomm.
- **Market Dominance**: Most widely used ISA in embedded systems (smartphones, IoT).
- **Cortex Families**:

| Family   | Target Use Case              | Example features                     |
| -------- | ---------------------------- | ------------------------------------ |
| Cortex-M | Microcontrollers (low-power) | Thumb2 ISA, no MMU/FPU               |
| Cortex-R | Real-time systems            | Hard real-time, safety-critical      |
| Cortex-A | Application processors       | High performance (e.g., smartphones) |

---

## Cortex-M3 Specifics
- **Key Features**:
  - 3-stage pipeline (Fetch, Decode, Execute).
  - Harvard architecture (separate data/instruction buses).
  - Thumb2 ISA (16/32-bit mixed instructions).
  - 32-bit registers, 8-zone Memory Protection Unit (MPU).
  - Up to 32 MHz clock, 1.25 DMIPS/MHz.
- **Use Cases**: STM32L152RE MCU, Apple A9 motion coprocessor.

---

## Thumb2 ISA
### Key Characteristics
- **Variable-Length Instructions**: Mix of 16-bit and 32-bit for code density.
- **Registers**:
  | **Register** | Alias | Purpose                          |
  |--------------|-------|----------------------------------|
  | R0-R12       | –     | General-purpose (R8-R12 require 32-bit instructions) |
  | R13          | SP    | Stack Pointer (banked MSP/PSP)   |
  | R14          | LR    | Link Register (stores return address) |
  | R15          | PC    | Program Counter                  |
- **Status Register (xPSR)**:
  - **Flags**: `N` (Negative), `Z` (Zero), `C` (Carry), `V` (Overflow).
  - **Modes**: Thread (user) vs. Handler (exception).

---

## Core Instructions
### Arithmetic & Logic
| **Instruction** | Example           | Description                     |
|------------------|-------------------|---------------------------------|
| `MOV`           | `MOV R0, R1`     | Move register/value             |
| `ADD`           | `ADD R0, R1, R2` | R0 = R1 + R2                    |
| `SUB`           | `SUB R0, R1, #5` | R0 = R1 - 5                     |
| `MUL`           | `MUL R0, R1, R2` | R0 = R1 × R2                    |
| `SDIV`          | `SDIV R0, R1, R2`| Signed division (R0 = R1 ÷ R2) |

### Memory Access
- **Endianness**: Little-endian (LSB at lower address).
- **Addressing Modes**:
  1. **Offset**: `LDR R0, [R1, #4]` (R0 = value at R1 + 4).
  2. **Pre-indexed**: `LDR R0, [R1, #4]!` (R1 += 4 after load).
  3. **Post-indexed**: `LDR R0, [R1], #4` (R1 += 4 after load).
- **Load/Store Examples**:
  ```assembly
  LDR R0, [R1]      ; Load word from R1 into R0
  STRH R2, [R3, #8] ; Store halfword from R2 to R3 + 8
  ```

---

## Control Flow
### Branches & Conditionals
| **Instruction** | Example           | Condition           |
|------------------|-------------------|---------------------|
| `B`             | `B loop`          | Unconditional jump  |
| `BEQ`           | `BEQ label`       | Branch if Z=1       |
| `CMP`           | `CMP R0, R1`      | Set flags for R0-R1 |
| `CBZ`           | `CBZ R0, exit`    | Branch if R0 == 0   |

### Loop Example (GCD Algorithm)
```assembly
MOV R0, #0x100       ; Load x (address 0x100) into R1
LDR R1, [R0]
LDR R2, [R0, #4]     ; Load y (address 0x104) into R2
loop:
  CMP R1, R2
  BEQ end
  BHI subtract_x     ; Branch if x > y
  SUB R2, R2, R1     ; y = y - x
  B loop
subtract_x:
  SUB R1, R1, R2     ; x = x - y
  B loop
end:
STR R1, [R0, #8]     ; Store result at 0x108
```

---

## Subroutine Management
### Calling Conventions
- **Branch with Link**: `BL func` (saves return address in LR).
- **Return**: `BX LR` or `POP {PC}`.
- **Nested Calls**: Save LR to the stack to prevent overwriting.
  ```assembly
  BL func1
  func1:
    PUSH {LR}        ; Save LR
    BL func2
    POP {PC}         ; Restore LR to PC
  ```

### Stack Usage
- **Push/Pop**:
  ```assembly
  PUSH {R0-R3, LR}   ; Save registers and LR
  POP {R0-R3, PC}    ; Restore and return
  ```
- **Stack Frame**:
  - Grows downward (higher to lower addresses).
  - Managed via SP (R13).

---

## ARM Cortex-M ABI (Application Binary Interface)
- **Caller-Saved Registers**: R0-R3 (arguments), R12, LR.
- **Callee-Saved Registers**: R4-R11, SP.
- **Return Values**: Stored in R0 (or R0-R1 for 64-bit).
- **Argument Passing**:
  - First 4 args in R0-R3; additional args on stack.
- **Example Function**:
  ```asm
  ; int foo(int a, int b)
  foo:
    MUL R0, R0, R0    ; a²
    MUL R1, R1, R1    ; b²
    ADD R0, R0, R1    ; R0 = a² + b²
    MOV R1, #3
    SDIV R0, R0, R1   ; R0 /= 3
    BX LR             ; Return
  ```

---
