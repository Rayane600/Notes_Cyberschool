


# Linux (Intel) Anti-Debugging Techniques

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
# Experimental Setup

## **1.The setup**
### **Linux (Intel) Experiments**

- **Test Environment:** Virtualized **Ubuntu 22.04.1 LTS (kernel 5.15.0-91-generic)** on **VMWare Workstation 16.2.5**.
- **Specs:** 2 vCPUs, 4GB RAM, 100GB disk.
- **Execution:**
    - Same methodology as Windows (1010 runs, 10 discarded).
    - Serial execution to minimize context switching.

### **Linux (ARM) Experiments**

- **Test Environment:** Virtualized **Ubuntu 22.04 LTS (kernel 6.1, arm64)** on **AWS t4g.medium node**.
- **Specs:** 2 vCPUs, 4GB RAM, 30GB SSD.
- **Execution:**
    - **Same methodology as above**.
    - Adaptations for ARM-specific behaviors (detailed below).

---

## **2.Experiment Implementation**

Each experiment applied an **anti-debugging technique** and measured execution performance. The **same base test program** was executed to compare against a version with anti-debugging enabled.

- **Linux (Intel) tests compiled with** `gcc` 11.4.0.
- **Linux (ARM) tests compiled with** `aarch64-linux-gnu-g++-11` 11.4.0.

After data collection:

- Logs were converted to **Pandas DataFrames**.
- Statistical analysis was performed using **Mann-Whitney U test** (median comparison) and **Levene test** (variance comparison).

### **ARM-Specific Adaptations**

- The **`INT3` instruction (breakpoint)** does not exist in ARM; instead, **`BRK` was used**, which halts execution permanently.
- **Single-Step Debugging (Trap Flag)** is **not modifiable in user mode** on ARM, affecting certain anti-debugging techniques.

---

## **3. Experiment Results**

For each test, execution time was recorded, and **three statistical analyses** were performed:

1. **Distribution graphs** for data visualization.
2. **Mann-Whitney U test** to check for median differences.
3. **Levene test** to compare variance.

### **Linux (Intel) Results**

- Many techniques had **consistent performance variance**.
- Certain methods, like **memory encryption checks**, showed **strong detection differences** from baseline.

### **Linux (ARM) Results**

- ARM had **significant implementation differences** due to the **lack of certain CPU features**.
- **INT3-equivalent tests were replaced with `SIGTRAP`**.
- Some tests, like **self-debugging**, failed due to ARM limitations.

---

## **4. Artifact Experiment**

A final experiment tested the **artifact’s ability** to detect anti-debugging techniques from collected performance data.

- Implemented in **Python 3.10.7**.
- Used **Levene’s test** to compare **new sample data** with pre-collected anti-debugging models.
- **Three sample execution durations** were tested:
    - **Fast sample (1000 iterations)**
    - **Medium sample (10,000 iterations)**
    - **Slow sample (100,000 iterations)**
- **Detection success varied** across techniques, with some working in fast/medium tests but failing in slow tests due to execution time variance.

---

## **Key points for Reimplementation**

### **General Implementation Notes**

- **Use a serial execution model** to prevent performance interference.
- **Compile appropriately** (`gcc` for Intel, `aarch64-linux-gnu-g++` for ARM).
- **Run at least 1010 iterations** to normalize cache behavior.
- **Ensure adaptation for architecture-specific constraints** (e.g., handling `INT3` differently on ARM).

### **Anti-Debugging Techniques Considerations**

- **Timing-based anti-debugging** is significantly different across platforms.
- **Exception-based methods (e.g., UnhandledExceptionFilter)** work well on Windows but have **Linux-specific implementation differences**.
- **Memory-based detection techniques** (e.g., `NtGlobalFlag`) are highly dependent on OS API access.

### **Detection and Evaluation**

- **Mann-Whitney U test** is effective for detecting median execution time differences.
- **Levene test** is useful for identifying variance in execution.
- **ARM has significant limitations** in debugging control, requiring alternative approaches.

---
# Summary of Research Achievements
## **1.General summary**

- The research **implemented 58 anti-debugging techniques** across different OS and architectures.
- A **dataset of performance metrics** for these techniques was generated.
- Performance data was used to create **statistical models** for each technique per OS and architecture.
- An **artifact** was developed to detect anti-debugging methods based on execution performance analysis.
- The artifact **successfully identified 27 out of 58 techniques** with some degree of sensitivity to execution duration variance.
- **7 of those 27 techniques** were found to be **insensitive to variance**, making them the most reliable detections.

### **General Observations**

- **Windows techniques** were the **least sensitive** to execution variance but often **unviable** due to API dependencies.
- **Linux (Intel) and Linux (ARM) techniques** generally fell in the middle, with varying levels of sensitivity.

---

## **2. Comparison of Anti-Debugging Techniques Across OS & Architectures**

### **Windows vs. Linux (Intel)**

- **Code Checksum Technique**:
    
    - Windows implementation had **high variance**, making it easily detectable.
    - Linux implementation was **consistent** with minimal variance, making detection harder.
    - The artifact successfully detected Windows checksum manipulation in **all samples**, while Linux was detected in **only one sample**.
- **Process Enumeration Technique**:
    
    - Similar across OS, but Windows showed **more variance** in execution times.
    - The artifact **only detected the technique in medium-duration tests** on Windows but **failed on Linux**.
- **Self-Debugging**:
    
    - Failed to be detected in any sample across both OS.
    - Declared **unviable for artifact-based detection**.

### **Linux (Intel) vs. Linux (ARM)**

- **Clock Anti-Debugging Techniques (e.g., `RDTSC`)**:
    
    - Linux (Intel) version was highly sensitive to execution duration variance.
    - **ARM implementation (`CNTVCT_EL0`)** was similarly sensitive but less reliable in detection.
    - **Artifact detected the ARM version only in the slow sample**.
- **Memory Encryption Checks**:
    
    - **Failed across both architectures**, meaning it was **unviable for detection**.
- **INT3 and `SIGTRAP` Scanning**:
    
    - Worked well on Intel but was **problematic on ARM** due to architectural differences.

---

## **3. Classification of Techniques Based on Sensitivity**

|Sensitivity Level|Description|Techniques|
|---|---|---|
|**Unviable**|Not detectable due to insufficient variance|Memory Encryption, Self-Debugging|
|**Very Sensitive**|Detected but highly impacted by execution variance|Clock-based techniques (`RDTSC`, `CNTVCT_EL0`)|
|**Mildly Sensitive**|Detectable with moderate variance sensitivity|Code Obfuscation, Process Enumeration|
|**Insensitive**|Detected consistently across all samples|Code Checksum (Windows)|

---

## **4. Future Work**

- **Enhancing Detection Models**: Improve statistical modeling for low-variance techniques.
- **Expanding Architectures**: Extend research to other architectures like **RISC-V, PowerPC, and MIPS**.
- **Kernel-Level Anti-Debugging**: Study kernel-mode anti-debugging techniques.

---

## **Key Takeaways for Reimplementation**

- **Techniques with high variance** (e.g., Windows checksum methods) are **easier to detect**.
- **Linux implementations are more stable**, requiring **better statistical models for detection**.
- **ARM presents significant challenges**, mainly due to:
    - **Lack of some x86 debugging instructions** (e.g., `INT3`).
    - **Different CPU cycle measurement techniques**.
- **The artifact is most effective for techniques that are insensitive to variance**.
