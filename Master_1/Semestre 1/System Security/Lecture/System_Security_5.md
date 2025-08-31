
### **Part I: Limited Direct Execution**
**CPU Virtualization**  
- The core idea is **time-sharing** the CPU, where multiple processes run for a short time each.
- **Challenges**:
  1. **Performance**: Implementing virtualization without excessive system overhead.
  2. **Control**: Running processes efficiently while retaining control over the CPU.

**Direct Execution Problems**:
1. How can the OS ensure that a program doesn’t misbehave?
2. How does the OS stop one process to switch to another?

---

### **Part II: Restricted Operations**
**Problem with Direct Execution**  
- **Advantage**: Fast, since the program runs natively on hardware.
- **Issue**: Processes need to perform restricted operations (I/O) without full system control.

**A Better Approach**  
- **Privileged vs Unprivileged Instructions**:  
  - **User processes** can execute **unprivileged instructions**.  
  - **Privileged instructions** (e.g., I/O, memory access) are controlled via the OS API.
  
**System Calls**  
- Programs access **OS services** using system calls, which function like API calls for the OS.

**Abstract Machine**  
- The **system call interface** acts as an abstract machine provided by the OS.
- The process **passes information** to the OS for operations. OS checks if the operation is permitted.

---

### **Kernel-Space vs User-Space**
- **User space** cannot access **kernel memory** (OS is trusted, applications are not).
- Kernel must ensure it doesn’t **blindly follow pointers** to user space to avoid system crashes.

**Hardware Execution Modes**  
- **User mode**: Restricted. Can’t issue I/O requests. Trying to do so raises an exception.
- **Kernel mode**: Full privileges, including I/O requests and restricted instructions.

---

### **Exceptions and System Calls**
- Exceptions: Change control flow when the processor state changes. Types:
  1. **Interrupts**: Async, signal from I/O device.
  2. **Traps**: Sync, intentional (e.g., system calls).
  3. **Faults**: Sync, recoverable error.
  4. **Aborts**: Sync, non-recoverable error.

**System Calls**  
- Execute via a **trap instruction**:
  - Switches control to the kernel and raises privileges.
  - After execution, returns to the user mode with a lower privilege.

---

### **Part III: Switching Between Processes**
**Mechanisms and Policies**  
- **Mechanism**: How the OS performs time multiplexing (e.g., context switches).
- **Policy**: Decides **which process** to run next (e.g., scheduling).

**Switching Problems**  
- **Crux**: How does the OS regain CPU control to switch between processes?

**Cooperative vs Non-Cooperative Approach**  
- **Cooperative**: Processes voluntarily give up CPU (via system calls).
- **Non-cooperative**: OS uses **timer interrupts** to forcefully stop processes.

**Context Switching**  
- **Scheduler**: Decides whether to switch the process.
- **Context Switch**: Saves the current process state and restores the next process state.

---

### **Part IV: System Calls in Unix**
**System Call Invocation in C/C++**  
- System calls are invoked almost directly using assembly support or through **library versions**.
- **Portable** through the **libc** interface.

**Unix Utilities**  
- Use `system()` function to execute shell commands from within a program.

**Examples of System Calls**  
- **getpid()**: Gets process ID.
- **write()**: Used in `printf()`.
- **exit()**: Exits the process.

**Architecture-Specific Syscalls**  
- On **x86_64 architecture**, `syscall` is the fast system call instruction.

**Transition from User to Kernel Space**  
- Different architectures have specific instructions for system calls (e.g., `int 0x80`, `sysenter`, `syscall`).

---

### **System Call Performance Optimization**
- System call performance is critical in applications (e.g., `gettimeofday()`).
- **Hardware**: New system call instructions (e.g., `syscall`).
- **Software**: **vDSO** (virtual dynamically linked shared object) reduces context switch overhead by mapping certain kernel functions directly to user space.

---