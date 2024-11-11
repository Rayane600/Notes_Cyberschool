#  One last fork
## Q1.
```
a=3
a=4
a=5
a=6
....
```
loops indefinitely. until fork fails then it decrements once and then fails again and so on and so forth.

# Execution Modes
## Q2.
TRUE => privileged hardware instructions can only be executed in kernel mode.

## Q3.

- Privileged. Executing an interruption is already handled in kernel mode.
- Unprivileged. it's a direct execution instruction since it would be too slow otherwise.

# Context Switching
## Q4.

False because whenever we jump to the kernel mode we run the scheduler and it may put the process into waiting.

## Q5.
- **Voluntary context switch** : By putting sleep() into our code.
- **Involuntary context switch** : The scheduler puts the process into waiting mode. (timer interrupt).
## Q6.

- B ==>
- The CPU save P registers in the Kernel Stack. ==>
- E ==> 
- the kernel store the kernel context of P in the PCB (rsp(kernel) ==>
- The kernel context of Q is loaded to the schedular ==>
- Read the kernel stack to restore the user context of Q
- C ==>
- A ==>
- D

# System Calls

## Q7.
True : A C program cannot directly invoke OS system calls. It must go through the C library (like `glibc` in Linux) to make system calls via a wrapper that interfaces with the kernel.

## Q8.
- **system** invokes a shell command, which uses underlying system calls but is not a direct system call itself.
- **fork** corresponds directly to the `fork()` system call.
- **exit** directly corresponds to `_exit()` or `exit_group()` in Linux.
- **strlen** is purely a C library function and does not involve a system call.
=>**strlen / system** do not correspond to a system call.

## Q9.
- **a: False** – `int n` can be executed by user-mode code, not just privileged OS code.
- **b: False** – `n` is used as an index for the IDT, not directly as an address for RIP.
- **c: True** – `n` is used as an index into the IDT.
- **d: False** – `int n` can be invoked by software, including user-mode processes, not just external hardware. The `int` instruction does not cause `int` . Instead implies Traps.

## Q10.
**a. How does the process obtain the address of the kernel instruction to jump to?**  
The process doesn't directly obtain the address. Instead, the CPU automatically switches to kernel mode via a hardware interrupt or trap (like `int n`). The interrupt syscall determines the appropriate kernel function.
**b. Where is the user-space context stored during the transition?**  
The user-space context (program counter, registers, etc.) is saved onto the process's kernel stack during the switch. This allows the kernel to resume execution in user mode after handling the system call.