
## Module 1: Introduction to Embedded Systems

### Embedded Systems (ES)
- Task-specific systems optimized for low power, reliability, real-time constraints, and minimal costs compared to General-Purpose (GP) systems.

### ES Application Domains:
- **Consumer Electronics**: Smartphones, TVs, gaming consoles.
- **Transportation**: Automotive, aerospace; safety-critical.
- **Industrial Systems**: Manufacturing, power grids.
- **Biomedical**: Pacemakers, medical imaging.

### Important Terms:
- **SRAM (Static RAM)**: Fast, volatile memory used for CPU caches; retains data as long as power is supplied.
- **DRAM (Dynamic RAM)**: Slower volatile memory used as main memory; requires constant refreshing.
- **Flash Memory**: Non-volatile storage, retains data without power, used in microcontrollers for storing program code.
- **EPROM (Erasable Programmable Read-Only Memory)**: Non-volatile memory that can be erased with ultraviolet light and reprogrammed.
- **EEPROM (Electrically Erasable Programmable Read-Only Memory)**: Non-volatile memory that can be erased and reprogrammed electrically.

---

## Module 2: ES Platforms Overview

### ASICs (Application Specific Integrated Circuits):
- Custom chips designed specifically for a particular application.
- High upfront cost, very efficient in power and performance.

### FPGAs (Field Programmable Gate Arrays):
- Reprogrammable logic devices configured via a "bitstream".
- **LUT (Lookup Table)**: Implements combinational logic by storing predefined outputs.

### System-on-Chip (SoC):
- Integrates processor cores, memory interfaces, and peripherals onto one chip.

### Microcontrollers (MCUs):
- Compact integrated circuits designed for embedded applications.
- Typically include CPU core, memory, and programmable peripherals.

---

## Module 3: ARM Microcontrollers (MCUs)

### ARM Architecture Overview:
- **RISC (Reduced Instruction Set Computer)**: Simple instructions, efficient pipeline execution.
- **Thumb2 Instruction Set**: Variable-length instructions (16/32 bits) to improve code density.

### Important ARM Registers:
- **PC (Program Counter)**: Points to the next instruction.
- **SP (Stack Pointer)**: Points to the top of the stack, holds temporary data.
- **LR (Link Register)**: Stores return address for subroutine calls.
- **xPSR (Program Status Register)**: Holds flags (e.g., N, Z, C, V) indicating CPU status after operations.

---

## Module 4: Toolchains

### Compilation and Linking:
- High-level languages (C/C++) translated to binary executables using cross-compilers (e.g., ARM GCC toolchain).
- **ELF (Executable and Linkable Format)**: Standard file format for executables, object code, and libraries.

### Important Terms in Toolchains:
- **Bootloader**: Initializes hardware and loads main application software upon startup.
- **Linker Script**: File specifying memory layout for code and data segments during linking.

### Memory Terms :
- **SRAM (Static RAM)**: Volatile memory for runtime data, stack, and heap.
- **VRAM (Video RAM)**: Specialized RAM dedicated to storing image data for graphics rendering.
- **EEPROM**: Used to store small amounts of persistent data; can be updated at runtime.

### Communication Interfaces and Protocols:
- **UART (Universal Asynchronous Receiver-Transmitter)**: Serial communication interface for data exchange.
- **H-UART (Hardware UART)**: UART implemented in hardware, handling communication more efficiently compared to software implementations.
- **SPI (Serial Peripheral Interface)**: High-speed, synchronous serial communication interface.
- **I²C (Inter-Integrated Circuit)**: Multi-device communication over two wires, typically for low-speed peripherals.

---

## Additional Key Concepts Defined:
- **DMA (Direct Memory Access)**: Transfers data directly between peripherals and memory, bypassing the CPU to improve efficiency.
- **GPIO (General-Purpose Input/Output)**: Pins on MCUs programmable for input or output.
- **ADC (Analog-to-Digital Converter)**: Converts analog signals (like temperature or sound) to digital signals understandable by the MCU.
- **DAC (Digital-to-Analog Converter)**: Converts digital signals from MCU to analog signals for actuators or audio outputs.

# EMS - CM5 & CM6 Notes: Peripherals, Interrupts & JTAG

##  General Concepts

###  What Are Peripherals?
- Peripherals allow an MCU to interact with the outside world via **pins**.
- Pins can be:
  - **GPIO**: General Purpose Input/Output.
  - **Alternate Functions**: Serial interfaces, ADC, DAC, display drivers, etc.

###  Multiplexing
- Each pin can be routed to multiple peripherals — via software-controlled **multiplexers (MUX)**.
- Only one function active per pin at a time.

---

## 🖲️ GPIO (General-Purpose I/O)

### As **Output**:
```c
// Set PB12 as output and drive high
GPIOB_MODER |= (1 << 24);      // Set mode
GPIOB_MODER &= ~(1 << 25);
GPIOB_ODR |= (1 << 12);        // Output Data Register
```

### As **Input with Pull-Up**:
```c
// Set PA8 as input with pull-up resistor
GPIOA_MODER &= ~((1 << 16) | (1 << 17));
GPIOA_PUPDR |= (1 << 16);
GPIOA_PUPDR &= ~(1 << 17);
```

> 📦 These registers are **memory-mapped**, meaning they live at fixed memory addresses outside the CPU core.

---

##  Memory Hierarchy & Buses

###  Common Bus Systems:
- **AHB (Advanced High-Performance Bus)**: High-speed memory & DMA transfers.
- **APB (Advanced Peripheral Bus)**: Slower, used for peripherals like UART/SPI.

###  Memory Access via DMA:
- **DMA (Direct Memory Access)**: Offloads repetitive memory transfers from the CPU.

---

##  Serial Communication Peripherals

###  UART (Universal Asynchronous Receiver/Transmitter)
- **Asynchronous** (no shared clock).
- Full duplex (TX + RX).
- Common for debugging, CLI interaction.
- Settings: `115200 8N1` = 115200 baud, 8 data bits, no parity, 1 stop bit.

>  **Baudrate**: Bits per second.

###  SPI (Serial Peripheral Interface)
- **Synchronous** (uses clock signal).
- 4 wires: `MOSI`, `MISO`, `SCK`, `CS`.
- Full duplex, high-speed, simple protocol.

### I2C (Inter-Integrated Circuit)
- 2 wires: `SDA` (data), `SCL` (clock).
- Supports multiple controllers/targets (7-bit addressing).
- Open-drain lines + pull-up resistors required.

>  **Open-Drain**: Pin can only pull low or float, useful for shared lines.

---

##  Terms Explained

| **Term**      | **Meaning**                                                                 |
|---------------|------------------------------------------------------------------------------|
| `SRAM`        | Static RAM. Fast, volatile memory inside MCUs (used for stack/data).        |
| `VRAM`        | Video RAM. Specialized memory for graphics (not common in basic MCUs).      |
| `EEPROM`      | Electrically Erasable PROM. Stores persistent data, slower than SRAM.       |
| `Flash`       | Non-volatile memory storing code (like `.text` section).                    |
| `MUX`         | Multiplexer. Routes one signal to many destinations under control.          |
| `Baudrate`    | Number of bits transmitted per second.                                      |
| `AHB/APB`     | ARM-standard buses for memory and peripheral access.                        |

---

# EMS - CM6: Interrupts and JTAG

##  What are Interrupts?
- An **interrupt** signals the CPU that an event needs immediate attention.
- Saves current state (context) and jumps to a handler: the **ISR (Interrupt Service Routine)**.

### Types:
- **IRQ**: Standard, maskable interrupt from peripherals.
- **NMI**: Non-maskable interrupt. Cannot be disabled, used for critical events.

---

##  NVIC (Nested Vectored Interrupt Controller)
- Prioritizes and routes interrupt requests.
- Stores handler addresses in a **vector table**.
- Takes ~12 cycles to enter an ISR, 6 to jump between ISRs.
- Can handle **nested interrupts** (ISR interrupted by higher priority ISR).

```c
// Vector table entry for UART
.word USART1_IRQHandler
```

---

##  Vector Table Example

```asm
.word _estack             // Top of stack
.word Reset_Handler       // Reset entry
.word NMI_Handler         // Non-maskable
.word HardFault_Handler   // HardFault
.word USART1_IRQHandler   // UART interrupt handler
```

---

##  ISR Best Practices
- Keep them **short** and **efficient**.
- Avoid loops, blocking calls.
- Use **volatile** or synchronization mechanisms (like semaphores) for shared variables.

---

##  JTAG & SWD (Debugging Interfaces)

### JTAG (Joint Test Action Group)
- Hardware debug interface.
- Used for:
  - Boundary scan (testing PCB connections).
  - Debugging MCU internal state (registers, breakpoints).

### Key Signals:
- `TCK`: Test Clock
- `TDI`: Data In
- `TDO`: Data Out
- `TMS`: Mode Select

### SWD (Serial Wire Debug)
- ARM-specific alternative to JTAG.
- Uses only 2 pins: `SWCLK`, `SWDIO`.
- Accesses same debug features using fewer pins.

---

##  Debugging Tools
- **OpenOCD**, **ST-Link**, **Segger J-Link**: Interface between PC and chip.
- **Ozone**, **GDB**, **STM32CubeIDE**: Software for debugging.

---

##  Cool Tools
- **JTAGulator**: Helps discover unknown debug/test points.
- **SpritesMods**: Hardware reverse engineering.
- **XIP (Execute in Place)**: Run code directly from external memory via SPI.



# ARM Thumb-2 Assembly Instruction Reference

## Instruction Reference Table

| **Instruction** | **Explanation / C Equivalent**                                     | **Notes**                                           |
|-----------------|--------------------------------------------------------------------|-----------------------------------------------------|
| `MOV Rd, Rn`    | `Rd = Rn;`                                                         | Copy register                                      |
| `MOV Rd, #imm`  | `Rd = imm;`                                                        | Load immediate value                               |
| `ADD Rd, Rn, Rm`| `Rd = Rn + Rm;`                                                    | Addition                                            |
| `ADD Rd, Rn, #imm`| `Rd = Rn + imm;`                                                 | Addition with constant                             |
| `SUB Rd, Rn, Rm`| `Rd = Rn - Rm;`                                                    | Subtraction                                         |
| `SUB Rd, Rn, #imm`| `Rd = Rn - imm;`                                                 | Subtraction with constant                          |
| `MUL Rd, Rn, Rm`| `Rd = Rn * Rm;`                                                    | Multiplication                                     |
| `SDIV Rd, Rn, Rm`| `Rd = Rn / Rm;` (signed)                                          | Division                                           |
| `UDIV Rd, Rn, Rm`| `Rd = Rn / Rm;` (unsigned)                                        | Division                                           |
| `CMP Rn, Rm`    | `if (Rn - Rm)` set flags                                           | Used for conditionals                              |
| `CMN Rn, Rm`    | `if (Rn + Rm)` set flags                                           | Compare Negative                                   |
| `TST Rn, Rm`    | `if (Rn & Rm)` set flags                                           | Bitwise AND test                                   |
| `AND Rd, Rn, Rm`| `Rd = Rn & Rm;`                                                    | Bitwise AND                                        |
| `ORR Rd, Rn, Rm`| `Rd = Rn | Rm;`                                                    | Bitwise OR                                         |
| `EOR Rd, Rn, Rm`| `Rd = Rn ^ Rm;`                                                    | Bitwise XOR                                        |
| `B label`       | `goto label;`                                                      | Unconditional branch                               |
| `BL label`      | `call function`                                                    | Branch with link (function call)                   |
| `BX Rm`         | `return;` / `jump to Rm`                                           | Branch to address in Rm                            |
| `PUSH {regs}`   | Save registers to stack                                            | e.g. `PUSH {r4, lr}`                               |
| `POP {regs}`    | Restore registers from stack                                       | e.g. `POP {r4, pc}`                                |
| `LDR Rt, [Rn, #offset]`| `Rt = *(Rn + offset);`                                     | Load from memory                                   |
| `STR Rt, [Rn, #offset]`| `*(Rn + offset) = Rt;`                                     | Store to memory                                    |
| `ADR Rd, label` | `Rd = &label;`                                                     | Get address of label                               |
| `LSL Rd, Rn, #imm`| `Rd = Rn << imm;`                                                | Logical shift left                                 |
| `LSR Rd, Rn, #imm`| `Rd = Rn >> imm;`                                                | Logical shift right                                |
| `ROR Rd, Rn, #imm`| `Rd = rotate_right(Rn, imm);`                                   | Rotate bits                                        |
| `NOP`           | No operation                                                       | Used for delays or alignment                       |
| `BKPT #imm`     | Breakpoint for debugging                                           |                                                     |

---

## Common C Code Translated to ARM Assembly

### 1. Simple Addition

```c
int a = 5;
int b = 10;
int c = a + b;
```

```asm
MOV R0, #5       ; a
MOV R1, #10      ; b
ADD R2, R0, R1   ; c = a + b
```

---

### 2. If-Else Statement

```c
if (a == b)
  c = 1;
else
  c = 0;
```

```asm
CMP R0, R1
BEQ label_true
MOV R2, #0       ; else
B label_end
label_true:
MOV R2, #1       ; then
label_end:
```

---

### 3. While Loop

```c
while (x > 0) {
  x--;
}
```

```asm
loop:
CMP R0, #0
BLE end
SUB R0, R0, #1
B loop
end:
```

---

### 4. Function Call

```c
int square(int x) {
  return x * x;
}
```

```asm
; assume input in R0
PUSH {LR}
MUL R0, R0, R0
POP {PC}
```

---

### 5. Array Access

```c
int arr[4];
arr[2] = 42;
```

```asm
LDR R0, =arr
MOV R1, #42
STR R1, [R0, #8]   ; 2 * 4 = 8 (each int = 4 bytes)
```

---

### 6. Return Value

```c
return a + b;
```

```asm
ADD R0, R0, R1   ; return value in R0
BX LR
```

---

## Tips

- Registers `R0`–`R3`: Used to pass arguments.
- `R0`: Always used for return value.
- `LR` (R14): Link Register, holds return address after a `BL`.
- `SP` (R13): Stack Pointer.
- Always `PUSH {LR}` before a `BL` inside a function, and `POP {PC}` to return.

