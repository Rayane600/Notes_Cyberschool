
---

# Debugging Tools Overview

## Key Tools
- **GDB**: A debugger for tracing program execution, breakpoints, memory inspection, etc.
- **Valgrind**: A memory checker used for dynamic analysis.
- **clang-tidy**: A static code analyzer for C/C++.
  
---

# Debug Methodology

## From Bugs to Failures
- **Defect**: Actual bug in the code.
- **Infection**: The incorrect code execution affects memory.
- **Failure**: Observable incorrect behavior.
- Example:
  ```c
  int a = foo(42);  // Defect: foo() returns 1
  int b = a - 1;    // Infection: b = 0
  int c = 1 / b;    // Failure: division by 0
  ```

## Types of Failures
- **Incorrect Behavior**: Not meeting specification.
- **Crash**: Termination (e.g., segmentation fault).
- **Memory Leak**: Memory not properly freed.
- **Non-deterministic Failure**: Unpredictable failures (often multi-threaded).
- **Heisenbug**: Disappears when using a debugger.

## Debug Methodology
- **Good Practice**:
  - Reproduce, simplify, trace, read, understand, and fix bugs.
- **Bad Practice**:
  - Random code changes or copying from the internet.

---

# GDB: GNU Debugger

## Features
- **Breaking**: Pause execution at certain points.
- **Single-stepping**: Step through the code line by line.
- **Value Tracking**: Inspect memory values during runtime.
- **Runtime Modifications**: Modify memory directly while debugging.
- **Disassembly**: View machine instructions.

## Why Use GDB Over `printf`
- No need for recompilation, easier high-level data visualization, memory inspection, handles multi-threading.

## GDB Setup
- Compile with debugging information using:
  ```sh
  gcc -Wall -Wextra -g3 -o hello hello.c
  ```

## GDB Commands
- **Basic Commands**:
  - Start GDB: `gdb ./hello`
  - Run program: `(gdb) run`
  - Set args: `(gdb) set args p1 p2`
  - Quit GDB: `(gdb) quit`
  
## Breakpoints and Watchpoints
- **Types**:
  - **Breakpoints**: Stop at a specific code location.
  - **Watchpoints**: Stop when a variable changes.
- **Commands**:
  - Set breakpoint: `(gdb) break main`
  - Continue to next breakpoint: `(gdb) continue`
  - Set watchpoint: `(gdb) watch x`

## Example Workflow
```sh
$ gdb --quiet ./hello
(gdb) break main
(gdb) run
(gdb) set args 2
(gdb) continue
(gdb) quit
```

---

# Valgrind: Memory Checker

## Key Features
- Detects **memory errors**, **uninitialized values**, **invalid free**, and **memory leaks**.
- Uses dynamic analysis by running programs in a simulated environment.
- Works well with binaries containing debug information.

## Valgrind Commands
- Run with memory checks:
  ```sh
  valgrind ./example
  ```
- **Memory Errors** Detected:
  - **Invalid Read/Write**: Access beyond allocated memory.
  - **Invalid Free**: Freeing an already freed area.
  - **Memory Leaks**: Leaked memory not freed at program termination.

## Example Errors
```c
#define N 10

int main() {
    int *tab = calloc(N, sizeof(int));
    int ret = tab[N];  // Invalid read beyond the allocated range
    free(tab);
    return 0;
}
```
Valgrind Output:
```
==5703== Invalid read of size 4
==5703== Address 0x4a94068 is 0 bytes after a block of size 40 alloc'd
```

---

# Additional Notes

## Debugging Techniques
- **Static Analysis**: Analyzes code without executing it (clang-tidy, cppcheck).
- **Dynamic Analysis**: Debuggers like GDB, Valgrind which analyze running code.
  
## GDB Commands Summary
- **Stepping**:
  - Step into functions: `(gdb) step`
  - Step over functions: `(gdb) next`
  - Display call stack: `(gdb) backtrace`
  
- **Memory Display**:
  - Print a variable: `(gdb) print x`
  - Print variable type: `(gdb) ptype x`
  - Display memory contents: `(gdb) x /10xb &i` (display 10 bytes from the address of `i`)

## Good Practices
- **Avoid Random Changes**: Understand the code and the bug.
- **Check Memory**: Use tools like Valgrind to find and fix memory issues.
- **Compile with Debug Info**: Use `-g` to make debugging easier.

---
