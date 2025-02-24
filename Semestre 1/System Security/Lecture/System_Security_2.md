# Introduction to OS

### Diversity of Operating Systems
- Popular operating systems include **Windows**, **macOS**, and various distributions of **Linux** (e.g., Ubuntu, Fedora, Debian).

---

### Computer System Components
1. **Hardware**: Provides basic computing resources like CPU, memory, and I/O devices.
2. **Operating System (OS)**: Controls and coordinates hardware use among applications and users.
3. **Application Programs**: Define how system resources are used to solve users’ computing problems (e.g., word processors, compilers, web browsers).
4. **Users**: People, machines, or other computers interacting with the system.

---

### What is an Operating System?
- An **OS** is system software that acts as an intermediary between the user and hardware.
- It simplifies hardware complexity and provides a simple interface for the user.
- **Main functions**: Manages resources efficiently (e.g., CPU, memory, I/O devices), runs user programs, and makes the computer more convenient to use.

**Goals of an Operating System**:
- Execute user programs.
- Ensure efficient and convenient use of the hardware.

---

### Abstract View of an Operating System
- **Hardware components** are at the bottom layer and are crucial to the system.
- The **Operating System** runs in **kernel mode**, where it has direct access to hardware and can execute all machine instructions.
- **User programs** and interfaces run in **user mode**, with restricted access to system resources.

---

### The OS as an Extended Machine
- The machine-level system is complex, especially for I/O operations.
- **Abstraction** is key, allowing users and programmers to work without worrying about hardware complexities (e.g., writing to files without managing the file system).
- Good abstractions:
  - Define and implement mechanisms for complex tasks.
  - Simplify problem-solving using those abstractions.

---

### The OS as a Resource Manager
- The OS **allocates resources** like the CPU, memory, and I/O devices in an orderly and controlled manner.
- **Resource management** involves **multiplexing** (sharing resources) in two ways:
  1. **Time multiplexing**: Programs take turns using resources like the CPU.
  2. **Space multiplexing**: Programs share resources simultaneously, with each program using part of the resource.

---

### Design Goals of an Operating System
#### 1. Abstraction
- Make the system convenient and easy to use.
#### 2. Performance
- Minimize overhead in terms of time and space.
#### 3. Protection
- Isolate processes from one another to prevent interference.
#### 4. Reliability
- Ensure the system runs without crashing.
#### 5. Security
- Protect against malicious applications, building on the foundation of process protection.
#### 6. Energy-efficiency
- Use system resources effectively to minimize energy consumption.
#### 7. Portability
- Make the OS adaptable to different hardware systems without requiring significant changes.

---

### Mechanisms and Policies in Operating Systems
- **Mechanisms** are low-level methods or protocols that implement specific functionality in the OS (e.g., time multiplexing).
- **Policies** are algorithms or decisions that the OS makes using those mechanisms (e.g., deciding which process runs next).
- The separation of mechanisms and policies allows **modularity**, enabling changes to policies without needing to modify mechanisms.

---
Here are some notes based on the section "Brief History" starting from part III in your document. You can paste these into your Obsidian vault:

---
# A brief History

## Early Operating Systems
- **OS as libraries**: Initially, operating systems were sets of libraries to assist developers with common functions, e.g., low-level I/O handling.
- **Batch Processing**: Early computers were not interactive, and tasks were grouped into "batches" for processing by a human operator.
- **Mainframes**: These large computers were housed in separate rooms and handled multiple jobs in groups.
## Era of Multiprogramming
- **Introduction of multiprogramming**: Improved CPU utilization by switching rapidly between jobs, especially due to slow I/O devices.
- **Memory protection**: Became critical to prevent programs from accessing each other’s memory.
## Advent of UNIX
- **UNIX development**: Influenced by Multics (MIT). Made systems simpler for programmers and developers, with a compiler for the C programming language.
- **Variants**: UNIX spread across companies with versions like SunOS (Sun Microsystems) and AIX (IBM).
- **Legal hurdles**: UNIX faced ownership disputes, and Windows gained popularity in the PC market.
## ACM Turing Award
- **ACM**: The Association for Computing Machinery presents the Turing Award annually, regarded as the "Nobel Prize" of computing.
- **Alan Turing**: The award is named after the British mathematician who contributed significantly to computer science.

## The Modern Era
- **Personal computers (PCs)**: Driven by machines like the Apple II and IBM PC, low-cost PCs became widely adopted.
- **PC OS limitations**: Early PC operating systems like DOS lacked memory protection, allowing poorly programmed applications to corrupt memory.

## Linux Revolution
- **Linus Torvalds**: Developed Linux based on UNIX but without using the original codebase to avoid legal issues.
- **Linux adoption**: Widely used by companies (Google, Amazon, Facebook) and became popular on desktops through Steve Jobs' NeXTStep, later incorporated into macOS. Also gained dominance in the smartphone market via Android.
---
# Use-Case: Virtualizing Memory

## Memory and Programs
- **Memory as an array**: Memory consists of an array of bytes, where each address stores data that can be accessed or updated by a program.
- **Program operations**: Programs store their data in memory using instructions such as *load* and *store*.
- **Instructions in memory**: Every program instruction also resides in memory, meaning memory is accessed during every instruction fetch.

## Virtual Memory Example
- **Program instance memory**: Multiple instances of a program may seem to use the same memory address, but they actually access their own private memory space.
- **Private virtual address space**: Each process operates in its private virtual address space, mapped by the OS to the physical memory of the machine.
- **Memory isolation**: A program’s memory space does not interfere with other programs or the OS.

## Memory in Early Systems
- **Early memory layout**: The OS would sit in a fixed area of memory, with a single program using the rest of the available memory.
- **Limitations**: Only one program could run at a time, making it inefficient in terms of multitasking.

## Multiprogramming and Time Sharing
- **Multiprogramming**: As machines evolved, the concept of running multiple programs simultaneously (multiprogramming) emerged.
- **Time sharing**: As demand increased, time sharing allowed users to interact with the system in real-time, with multiple programs sharing CPU time.

## Memory Sharing
- **Process memory partitions**: Physical memory is divided between processes, with each process receiving its own space.
- **Protection concerns**: Processes must be isolated from one another to prevent them from reading or writing to each other’s memory.

## Failing Approach (Time Sharing)
- **Basic time sharing model**: An early attempt at time sharing involved switching between processes by saving and loading their entire state, including memory, which was slow and impractical.

## Principle of Isolation
- **Memory isolation**: The OS isolates processes to prevent one from affecting another, ensuring system reliability.
- **Microkernel approach**: Some modern OS designs, such as microkernels, further isolate parts of the OS itself for increased reliability.

## Address Space Virtualization
- **Address space components**: A process’s address space includes:
  - **Code**: The instructions executed by the program.
  - **Stack**: Temporary data used during execution.
  - **Heap**: Dynamically allocated memory.
- **OS role**: The OS virtualizes memory, allowing processes to believe they have access to a large, continuous address space, even though physical memory is finite.
- **Address translation**: When a process tries to access memory, the OS translates the virtual address to a physical address via the MMU.
## The MMU
### Key Functions of the MMU:

1. **Address Translation**:
    
    - The MMU translates virtual addresses, used by applications, into physical addresses, used by the hardware (RAM).
    - This allows programs to operate as if they have access to a large, contiguous block of memory, even though the actual physical memory may be fragmented.
2. **Memory Protection**:
    
    - The MMU enforces memory protection by ensuring that a process can only access the memory allocated to it.
    - If a process tries to access memory outside its allocated space, the MMU triggers a **segmentation fault** or **page fault**, which the operating system handles.
3. **Paging and Segmentation**:
    
    - The MMU supports **paging**, which divides virtual memory into fixed-size blocks called **pages** and maps them to physical memory.
    - It can also support **segmentation**, where memory is divided into variable-length segments.
4. **Caching Control**:
    
    - Some MMUs manage how certain memory regions are cached, providing control over how data is stored and retrieved for faster access.
5. **Access Permissions**:
    
    - The MMU defines read, write, and execute permissions for different sections of memory, ensuring that only authorized processes can modify or execute certain parts of memory.

### Importance of the MMU:

- **Efficiency**: It allows multiple processes to share the same physical memory without interfering with each other.
- **Security**: By isolating the memory space of processes, it prevents unauthorized access or modification of memory.
- **Flexibility**: Programs can use larger memory spaces (virtual memory) than the actual available physical memory through the use of **paging** and **swapping**.


---

