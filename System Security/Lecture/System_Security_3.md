
## System Security - Complete Notes with Examples

### Part 1: **The Abstraction: The Process**

#### What is a Process?
- A process is a **running instance** of a program.
- **Programs** are static code stored on disk, while **processes** are actively executing.
- **Multiple processes** can appear to run at once due to time-sharing, even though the system might only have a few CPUs.

#### **Time Sharing vs. Space Sharing**
- **Time Sharing**: The CPU is switched between multiple processes, giving each a time slice.
- **Space Sharing**: Resources like disk space or memory are divided among processes.

##### Example:
- When you’re editing a document while listening to music, the OS shares the CPU between your text editor and music player.

#### **Process States**
1. **Running**: The process is executing instructions on the CPU.
2. **Ready**: The process is ready to run, but the OS has scheduled another process.
3. **Blocked**: The process is waiting for an event (e.g., I/O operation) and cannot continue.

##### Example:
- A video player may be in the **blocked** state while waiting for data to load from the disk.

#### **Process State Transitions**
- Moving from **ready to running** means the process has been scheduled.
- Moving from **running to ready** means the process has been **descheduled**.

---

### Part 2: **Machine State**

#### **Machine State Definition**
- **Machine State** is what the program can read or modify while it’s running.
  
#### Components of Machine State:
- **Address Space**: The memory the process can access.
- **Registers**: The CPU’s temporary storage, including:
  - **Program Counter (PC)**: Points to the current instruction being executed.
  - **Stack Pointer (SP)**: Manages the function call stack for parameters, variables, and return addresses.
- **I/O Information**: Files or network connections the process has open.

##### Example:
- The process uses its **address space** to store code and data, while registers like **PC** help track execution.

#### **Machine State Initialization**
1. The OS allocates memory space.
2. Code and data are loaded into memory from disk.
3. The OS initializes registers (PC, SP) and opens basic I/O files like **stdin**, **stdout**, and **stderr**.

##### Example:
- When running a command in a shell, the OS loads the program code into memory and initializes these components before execution begins.

---

### Part 3: **Data Structures: PCB**

#### **Process Control Block (PCB)**
- The **PCB** is a data structure that stores information about each process, including:
  - Process state (running, ready, blocked).
  - Program counter (PC), stack pointer (SP).
  - Registers, address space.
  - List of open files, process ID (PID).

#### **Process List (Task List)**
- The OS keeps a **process list** to track all processes:
  - Processes that are running.
  - Processes that are ready.
  - Blocked processes.

---

### Part 4: **Proc File System (procfs)**

#### **The procfs Virtual File System**
- `/proc` is a **virtual file system** that stores system and process information.
- It’s generated dynamically when the system boots.

##### Important Directories:
1. **/proc/meminfo**: Shows how memory is managed by the kernel.
2. **/proc/[PID]/cmdline**: Command-line arguments of a process.
3. **/proc/[PID]/status**: Human-readable process status.

##### Example:
```bash
cat /proc/1234/status
```
This command shows the status of the process with PID 1234.

---

### Part 5: **Process API**

#### **Common Process Operations**
1. **Create**: OS creates a process when you run a command or open an application.
2. **Destroy**: Terminate a process either naturally (by completion) or forcefully (using `kill()`).
3. **Wait**: A parent process waits for its child to finish.
4. **Control**: Processes can be paused (`SIGSTOP`) or resumed (`SIGCONT`).
5. **Status**: Retrieve information about the process’s runtime state.

##### Example:
- Use the `wait()` system call to pause a parent process until its child completes:
```c
wait(NULL);
```

#### **The `fork()` System Call**
- `fork()` creates a new process that is a near-exact copy of the parent.
- Both parent and child processes share the same code but have separate memory spaces.

##### Example:
```c
pid_t pid = fork();
if (pid == 0) {
    printf("Child process\n");
} else {
    printf("Parent process\n");
}
```
This prints both "Child process" and "Parent process".

#### **The `exec()` System Call**
- **`exec()`** replaces the current process with a new program.
- Variants include `execvp()`, `execv()`, `execlp()`, etc.
- A successful `exec()` call never returns; it transforms the current process into the new program.

##### Example:
```c
execl("/bin/ls", "ls", NULL);
```
This replaces the current process with the `ls` command.

#### **The `kill()` System Call**
- Sends signals to processes:
  - **SIGKILL (9)**: Forcefully kills a process.
  - **SIGTERM (15)**: Politely asks a process to terminate.
  - **SIGSTOP (19)**: Pauses a process.
  - **SIGCONT (18)**: Resumes a paused process.

##### Example:
```bash
kill -9 4567
```
This command forcefully terminates the process with PID 4567.

---

### Part 6: **Format Highlights (binfmt)**

#### **Binary Formats**
- The OS must understand the format of executable files.
- It looks for a **magic number** in the first 256 bytes of the file to determine how to execute it.
- If it cannot recognize the format, it returns an error like `ENOEXEC`.

#### **Shebang (`#!/path/to/interpreter`)**
- The **shebang** line tells the OS which interpreter to use to run a script.

##### Example:
```bash
#!/usr/bin/python3
print("Hello, World!")
```
This script uses Python to execute because of the shebang.

#### **Script Execution Without Shebang**
- If a script lacks a shebang, the shell attempts to interpret it as a shell script by default.

##### Example:
- A `.sh` script without a shebang still runs because the shell retries execution as a shell script.

---

### Part 7: **Running a Program**

#### **Shell Workflow**
1. The user types a command into the shell.
2. The shell creates a child process using `fork()`.
3. The child process runs the command with `exec()`.
4. The shell waits for the child to finish with `wait()`.

#### **Output Redirection**
- The shell redirects output by closing the current file descriptor and opening a new one before calling `exec()`.

##### Example:
```bash
ls > output.txt
```
This redirects the output of the `ls` command to `output.txt`.

#### **Pipes**
- Pipes connect the output of one process to the input of another.

##### Example:
```bash
ps aux | grep bash
```
This pipes the output of `ps aux` into `grep` to filter for processes containing "bash".

---

