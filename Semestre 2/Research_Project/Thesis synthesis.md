


### **Linux (Intel) Anti-Debugging Techniques**

#### **1. Exception-based Techniques**

- **SIGTRAP Handling:**
    - Linux handles exceptions using **signals** rather than structured exception handling like Windows.
    - A process can register a **SIGTRAP handler** to detect whether the debugger intercepts the signal. If the handler executes, no debugger is present; otherwise, the program detects debugging.
- **Trap Flag (EFLAGS Register):**
    - The **Trap Flag** enables single-stepping execution. If a debugger is attached, it intercepts the generated **SIGTRAP** signal.
    - The program can determine debugger presence based on whether the signal is handled internally or intercepted.

#### **2. Scanning-based Techniques**

- **/proc/self/status File Check (TracerPid):**
    - In Linux, a running process can check **/proc/self/status**, where the `TracerPid` field indicates whether the process is being debugged.
    - A non-zero `TracerPid` value confirms debugger presence.
- **INT3 Instruction Scanning:**
    - Debuggers use `INT3` (`0xCC`) as breakpoints.
    - The program scans its memory for **unexpected occurrences** of `0xCC`.
    - If found, it assumes a debugger has modified the code.

#### **3. Timing-based Techniques**

- **RDTSC & Clock Measurement:**
    - Similar to Windows, timing-based techniques use **CPU cycle counters** to detect slowdowns caused by breakpoints.
    - On Linux, timestamps are obtained via:
        - `clock()`
        - `time()`
        - `clock_gettime()`
    - If execution time is significantly longer than expected, a debugger may be interfering.

#### **4. Miscellaneous Techniques**

- **Self-Debugging via ptrace()**
    - The process **forks a child**, which then attaches to the parent using `ptrace()`.
    - Since **only one debugger can attach to a process at a time**, this prevents other debuggers from interfering.
- **Memory Encryption:**
    - The program **encrypts its own memory** using OpenSSL AES, making it harder to analyze in a debugger.
    - This technique imposes a high **performance overhead** but protects sensitive data.

---

### **Linux (ARM) Anti-Debugging Techniques**

ARM differs from Intel in instruction set and debugging mechanisms, requiring modified techniques.

#### **1. Exception-based Techniques**

- **SIGTRAP Handling (Alternative to INT3)**
    - The **BRK instruction (`0xD4200000`)** is used instead of `INT3` for breakpoints.
    - Since **Linux ARM does not handle BRK like Intel’s INT3**, the program explicitly raises **SIGTRAP** using `raise(SIGTRAP)`.
- **Single-Step Debugging Detection**
    - ARM has a **Single-Step (SS) Flag** in the PSTATE register.
    - However, **this flag cannot be modified in user mode**, making the technique impractical.

#### **2. Scanning-based Techniques**

- **TracerPid Check (Same as Linux Intel)**
    - The `/proc/self/status` file is scanned for `TracerPid`, indicating debugger presence.
- **BRK Instruction Scanning**
    - Instead of `INT3`, ARM uses **BRK**, and the program scans for unexpected `BRK` instructions in its own memory.

#### **3. Timing-based Techniques**

- **CNTVCT_EL0 Register (ARM Equivalent of RDTSC)**
    - ARM lacks `RDTSC`, but uses `CNTVCT_EL0`, a high-precision cycle counter.
    - The program measures cycle differences before and after execution to detect debugger-induced delays.

#### **4. Miscellaneous Techniques**

- **Self-Debugging (Same as Linux Intel)**
    - A child process `ptrace()`s the parent to block external debuggers.
- **Memory Encryption**
    - The program encrypts **sensitive memory sections** to prevent debugger analysis.

---