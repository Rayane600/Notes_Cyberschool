# EMS - Toolchains & Embedded Memory Management

---

## Toolchains Overview
- **Purpose**: Translate high-level code (C/C++) to executable binaries for target architectures.
- **Cross-Compiling**:
  - Compile for a different architecture (e.g., x86_64 → ARM Thumb2).
  - **GCC Flags**:
    ```bash
    arm-none-eabi-gcc -mcpu=cortex-m3 -mthumb -c main.c -o main.o
    ```
  - Produces ELF files with Thumb2 instructions.

---

## ELF File Format
### Structure
- **Three Main Parts**:
  1. **Program Header Table**: Execution metadata.
  2. **Sections**: Code/data (`.text`, `.data`, `.bss`, `.rodata`, etc.).
  3. **Section Header Table**: Section descriptions.
- **Key Sections**:

| Section   | Contents                                   |
| --------- | ------------------------------------------ |
| `.text`   | Executable instructions                    |
| `.data`   | Initialized global variables               |
| `.bss`    | Uninitialized variables (zeroed)           |
| `.rodata` | Read-only data (e.g., strings)             |
| `.symtab` | Symbol table (function/variable addresses) |

### Tools for Inspection
- `arm-none-eabi-readelf -S`: View sections.
- `arm-none-eabi-readelf -s`: View symbols.

---

## Memory Organization in MCUs
### Key Differences from GP Systems
- **No MMU**: Direct physical memory access.
- **Memory Types**:
  - **Flash (NVM)**: Stores `.text`, `.data` (initialized), `.rodata`.
  - **SRAM**: Stores `.data` (runtime copy), `.bss`, stack, heap.
- **Example (STM32L152RE)**:
  - **Flash**: 512 KB.
  - **SRAM**: 80 KB.

### Memory Layout Example
```
FLASH (0x08000000)
├── Vector Table
├── .text (code)
├── .rodata (constants)
└── .data (initial values)

SRAM (0x20000000)
├── .data (runtime copy)
├── .bss (zero-initialized)
├── Heap
└── Stack
```

---

## Linker Scripts
### Key Directives
1. **`ENTRY`**: Specifies the first instruction (e.g., `Reset_Handler`).
2. **`MEMORY`**: Defines memory regions.
   ```ld
   MEMORY {
     FLASH (rx) : ORIGIN = 0x08000000, LENGTH = 64K
     SRAM (rwx) : ORIGIN = 0x20000000, LENGTH = 20K
   }
   ```
3. **`SECTIONS`**: Maps input sections to memory regions.
   ```ld
   SECTIONS {
     .text : { *(.text*) } > FLASH
     .data : { 
       _data_start = .;
       *(.data*)
       _data_end = .;
     } > SRAM AT> FLASH
     .bss : { *(.bss*) } > SRAM
   }
   ```

### Symbol Definitions
- `_data_start`, `_data_end`: Used in startup code to copy `.data` from Flash to SRAM.

---

## Cortex-M3 Boot Sequence
4. **Power-On Reset**:
   - Load initial stack pointer (SP) from `0x00000000`.
   - Jump to `Reset_Handler` (address at `0x00000004`).
5. **Reset Handler Tasks**:
   - Copy `.data` from Flash to SRAM.
   - Zero-initialize `.bss`.
   - Call `libc_init_array` (C runtime initialization).
   - Jump to `main()`.

### Startup Code (Simplified)
```assembly
Reset_Handler:
  // Copy .data from FLASH to SRAM
  ldr r0, =_data_start
  ldr r1, =_data_end
  ldr r2, =_data_load
  copy_loop:
    cmp r0, r1
    ldrlt r3, [r2], #4
    strlt r3, [r0], #4
    blt copy_loop

  // Zero .bss
  ldr r0, =_bss_start
  ldr r1, =_bss_end
  mov r2, #0
  zero_loop:
    cmp r0, r1
    strlt r2, [r0], #4
    blt zero_loop

  // Jump to main
  bl main
```
