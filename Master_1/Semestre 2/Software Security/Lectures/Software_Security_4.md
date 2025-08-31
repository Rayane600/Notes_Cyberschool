
# Video of the course:
https://www.youtube.com/watch?v=FY9SbqTO5GQ&ab_channel=linux.conf.au
# Notes
## **1. Introduction**

- **Why is C dangerous?**  
    C is one of the most widely used programming languages for system software, including the Linux kernel. However, it was designed decades ago when security wasn't the primary concern. It operates at a low level, providing **direct memory access**, which makes it fast but also prone to errors that attackers can exploit.
    
- **What is the Linux Kernel Self-Protection Project (KSPP)?**  
    KSPP is a community-driven initiative to make the Linux kernel resilient against attacks. Instead of just relying on userspace security mechanisms like firewalls or antivirus software, the goal is to **harden the kernel itself** against common exploits.
    

---

## **2. Why is C Like a Fancy Assembler?**

- Unlike modern languages like Rust or Python, **C doesn’t have built-in memory safety**.
- The **Linux kernel needs to be fast**, so it avoids extra layers of abstraction.
- **Many operations in C are close to machine code**:
    - Direct memory access
    - Pointer arithmetic
    - No built-in protection against misuse
- **Undefined behavior** in C allows dangerous exploits:
    - **Uninitialized variables**: Can contain garbage data or sensitive information.
    - **Void pointers**: Can be cast to any type, making it easy to call functions in unintended ways.
    - **Unsafe memory functions**: `memcpy()`, `strcpy()`, etc., can write past memory buffers.

---

## **3. The Problem with Variable Length Arrays (VLAs)**

- **What is a VLA?**  
    A **Variable Length Array** is an array where the size is determined at runtime rather than compile time.
    
    ```c
    int size = get_user_input();  // User provides size
    char buffer[size];  // Allocated dynamically on the stack
    ```
    
- **Why are VLAs dangerous?**
    
    - If the size is too large, it can **exhaust the stack** and cause a crash (stack overflow).
    - An attacker can **craft inputs** to manipulate buffer sizes and overwrite adjacent memory.
    - VLAs introduce **extra CPU instructions**, making execution **slower**.
- **Better alternatives to VLAs:**
    
    - Use **fixed-size arrays** whenever possible.
    - Allocate memory dynamically using **kmalloc()** (kernel equivalent of `malloc()`).

---

## **4. Switch Case Fall-Through: A Hidden Bug**

- In C, a missing `break;` statement in a `switch` can lead to **unintended execution**:
    
    ```c
    switch (x) {
        case 1:
            do_something();
        case 2:  // No break, so execution "falls through"!
            do_something_else();
    }
    ```
    
    - This can create subtle logic errors that **attackers might exploit**.
    - Common mistake categorized as **CWE-484**.
- **Fixing it:**
    
    - Use `-Wimplicit-fallthrough` compiler flag to detect missing breaks.
    - Explicitly mark fall-through cases using:
        
        ```c
        case 1:
            do_something();
            /* fall through */
        case 2:
            do_something_else();
        ```
        

---

## **5. Always-Initialized Variables**

- **Why is this a problem?**
    
    - **Uninitialized variables** can contain arbitrary memory data, leading to **information leaks** (CWE-200).
    - Attackers can **control the stack state** and use uninitialized variables to influence program behavior.
- **Example of dangerous code:**
    
    ```c
    int x;
    if (y) x = 10;
    printf("%d\n", x);  // x might be uninitialized!
    ```
    
- **How to fix it?**
    
    - Use compiler options:
        - **GCC:** `-finit-local-vars` (not yet in the mainline kernel).
        - **Clang:** `-fsanitize=init-local`.
    - Kernel has a **STRUCTLEAK plugin** to enforce initialization of sensitive data.

---

## **6. Arithmetic Overflow Detection**

- **Integer overflows** happen when a variable exceeds its maximum value.
    
    ```c
    int x = INT_MAX;  // 2147483647
    x = x + 1;  // Wraps around to -2147483648!
    ```
    
- **Exploiting integer overflows:**
    
    - Attackers can manipulate values to **bypass security checks**.
    - Example: If an allocation size overflows to a small value, an attacker can request a large buffer but receive a tiny one, allowing an **out-of-bounds memory write**.
- **How to prevent it?**
    
    - **GCC**: `-fsanitize=signed-integer-overflow`
    - **Clang**: Can detect both **signed and unsigned overflows**.

---

## **7. Bounds Checking**

- **Why does C allow writing past array bounds?**
    
    - C doesn’t track the size of arrays at runtime.
    - If you write beyond an array, it might overwrite **important data or function pointers**.
- **Unsafe string functions:**
    
    - `strcpy()` **doesn’t check** destination size.
    - `sprintf()` **doesn’t limit** how much it writes.
- **Safe replacements:**
    
    - `strscpy()` instead of `strcpy()`
    - `scnprintf()` instead of `sprintf()`
    - Careful manual bounds checking in `memcpy()`

---

## **8. Memory Tagging: Hardware-Level Protection**

- **What is memory tagging?**
    
    - Some modern CPUs support **hardware-assisted memory safety**.
    - Each memory region gets a **unique tag** to prevent overflows.
- **Examples:**
    
    - **ARM Memory Tagging Extension (MTE)**
    - **Intel Control Flow Enforcement Technology (CET)**
- **Impact:** These technologies can detect **out-of-bounds writes** and prevent **use-after-free attacks**.
    

---

## **9. Control Flow Integrity (CFI)**

- **What is CFI?**
    
    - It prevents attackers from **redirecting program execution** by modifying function pointers.
- **Forward-edge protection** (for function pointers):
    
    - Clang’s `-fsanitize=cfi` checks function calls **match expected types**.
- **Backward-edge protection** (for return addresses):
    
    - **Safe Stack:** Uses separate stacks for safe/unsafe data.
    - **Shadow Call Stack:** Stores return addresses in a dedicated memory region.

---

## **10. Where is the Linux Kernel Now?**

- **VLAs**: **Eradicated in kernel 4.20 (Dec 2018)!**
- **Switch case fall-through fixes**: **232 cases remaining out of 2311**.
- **Arithmetic overflow detection**: **Implemented for memory allocations**.
- **CFI support**: Available in **Android** but needs full kernel integration.

---

## **11. Challenges in Kernel Security Development**

1. **Cultural challenges:**
    
    - Kernel development is **conservative** – changes require **strong justification**.
    - Security fixes must not **hurt performance**.
2. **Technical challenges:**
    
    - The kernel is **huge** (~30 million lines of C code).
    - Compatibility issues between **GCC and Clang**.
3. **Resource challenges:**
    
    - Not enough **reviewers, testers, backporters**.

---

## **12. Conclusion**

- The **Linux kernel is improving** its security, but challenges remain.
    
- **Main takeaways:**
    
    - **Avoid VLAs**.
    - **Always initialize variables**.
    - **Use safer functions** (`strscpy()`, `scnprintf()`).
    - **Push for better compiler and hardware protections**.
- **How to contribute?**
    
    - Join the **Kernel Self-Protection Project (KSPP)**.
    - Help with **code reviews** and **testing**.
