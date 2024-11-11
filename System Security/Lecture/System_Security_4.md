
## System Security - Lecture Notes with Examples

### Part I: **Files and Directories**

#### **Files**
- Files are the fundamental way that persistent data is stored on a system.
- In Unix systems, **everything is treated as a file**, even devices and sockets.
- Files are organized as a **linear array of bytes**.
  - High-level: Simple sequence of bytes.
  - Low-level: Mapped to blocks on a storage device.

##### Example:
- In a Unix-based system, `/dev/sda1` (a hard disk partition) is treated as a file, and you can interact with it using standard file operations like read and write.

#### **Files vs. Memory**
- **Memory (RAM)**: Access is by direct address (e.g., 0x100).
- **Files**: Access is relative to the directory and requires opening/closing.
  - File systems maintain a **file pointer** that tracks the current position in the file.

##### Example:
- A program that reads a file uses a **file pointer** to know where in the file it is currently reading or writing.

#### **Directories**
- **Directories** are files with a type **directory** that contain a list of files, including other directories.
- Directories enable a hierarchical structure.
- Special directories:
  - `/`: Root directory.
  - `.`: Current directory.
  - `..`: Parent directory.

##### Example:
- The `/bin` directory contains essential system binaries, like `/bin/ls` or `/bin/cp`.

---

### Part II: **File System Constructs**

#### **File System**
- A file system organizes files on a storage device.
- Files have:
  - **High-level identifiers**: Names given by users.
  - **Low-level identifiers**: Unique inode numbers.

##### Example:
- Running `ls -i` in Linux shows the **inode number** for each file, which is used internally by the OS.

#### **Inodes and Metadata**
- An **inode** contains metadata about the file, including:
  - File type.
  - File size.
  - Ownership (UID, GID).
  - Access control (permissions).
  - Access timestamps.
  
##### Example:
```bash
stat myfile.txt
```
This command displays the inode information for `myfile.txt`, including its size, ownership, and access times.

#### **Blocks and Superblocks**
- Disks are divided into **blocks** (typically 4 KB), which store file data.
  - **Superblock**: Stores file system metadata (e.g., block counts, inode counts).
  - **Block Bitmap**: Tracks free/used blocks.
  - **Data Blocks**: Where actual file data is stored.

##### Example:
- A superblock might contain information like the total number of inodes and blocks, which the system uses to manage disk space.

---

### Part III: **File Descriptors**

#### **File Descriptors**
- A **file descriptor** is an integer representing an open file.
  - Returned by `open()` and used by system calls like `read()` and `write()`.
  - A file descriptor acts like a pointer to the file.

##### Example:
```c
int fd = open("file.txt", O_RDONLY);
```
This code opens `file.txt` for reading and returns a file descriptor, which can then be used for further operations.

#### **Special File Descriptors**
- Unix provides three standard file descriptors:
  - **0**: Standard input.
  - **1**: Standard output.
  - **2**: Standard error.

##### Example:
- Redirecting output to a file uses file descriptor `1`:
```bash
ls > output.txt
```
Here, file descriptor `1` (stdout) is redirected to `output.txt`.

#### **Open File Table (OFT)**
- The OS tracks open files in the **Open File Table** (OFT), which includes:
  - Reference count.
  - Current read/write offset.
  - Access control (read/write permissions).

#### **Per-Process File Table**
- Each process has its own **file descriptor table** that points to entries in the system-wide **Open File Table**.
- When a file is opened, an entry is created in both the per-process table and the OFT.

---

### Part IV: **File System Interface**

#### **File Operations**
- Common file operations:
  - **Open**: Open a file for reading, writing, or both.
  - **Close**: Close an open file descriptor.
  - **Read**: Read data from a file.
  - **Write**: Write data to a file.
  - **lseek**: Move the file pointer to a new position.
  
##### Example:
```c
int fd = open("myfile.txt", O_RDONLY);
char buf[100];
int bytesRead = read(fd, buf, 100);
close(fd);
```
This code opens `myfile.txt`, reads 100 bytes into `buf`, and then closes the file.

#### **System Call: `creat()`**
- The `creat()` system call creates a new file or truncates an existing file to 0 bytes.
  
##### Example:
```c
int fd = creat("newfile.txt", S_IRUSR | S_IWUSR);
```
This creates a new file with read/write permissions for the user.

#### **System Call: `open()`**
- Opens a file and returns a file descriptor.
- Flags determine whether the file is opened for reading, writing, or both.
  
##### Example:
```c
int fd = open("myfile.txt", O_CREAT | O_WRONLY | O_TRUNC, S_IRUSR | S_IWUSR);
```
This opens `myfile.txt` for writing, creates it if it doesn’t exist, and truncates it if it does.

#### **System Call: `read()` and `write()`**
- **`read()`**: Reads data from a file into a buffer.
- **`write()`**: Writes data from a buffer to a file.

##### Example:
```c
ssize_t n = read(fd, buffer, 100);
```
This reads up to 100 bytes into the `buffer`.

#### **System Call: `lseek()`**
- Moves the file pointer to a specific location within the file.
  
##### Example:
```c
lseek(fd, 0, SEEK_SET); // Move file pointer to the beginning
```

#### **System Call: `dup()`**
- Duplicates a file descriptor.
  
##### Example:
```c
int newfd = dup(fd);
```
This creates a new file descriptor pointing to the same file as `fd`.

#### **System Call: `stat()` and `fstat()`**
- Retrieves metadata about a file.

##### Example:
```c
stat("myfile.txt", &statbuffer);
```
This retrieves metadata from `myfile.txt`.

---

1. **Directory Execute Permission (`x`)**:
   - Without **execute permission** on a directory, you cannot traverse into that directory, so you won't be able to see its contents, regardless of whether you have read or write permissions on the directory.
   - If you try `ls -l` without execute permission on the directory, you will get a **"Permission denied"** error:
     ```
     ls: cannot access 'file': Permission denied
     ```

2. **Directory Read Permission (`r`)**:
   - Read permission allows you to list the directory's contents (i.e., the filenames), but without execute permission, you cannot access details about the files, such as their size, permissions, owner, etc.
   - If you have **read but not execute** permission on the directory, you can see the filenames but will not be able to get details with `ls -l`. Attempting to list file details will result in "Permission denied" errors.

3. **File Read Permission (`r`)**:
   - If you have **execute permission** on the directory but **no read permission** on the file itself, the file will be listed by `ls -l`, but the file's contents cannot be read. However, you can still see the file’s details, such as its size, permissions, and modification time.

---

### Part V: **Directory Operations**

#### **Directory Operations**
- **Create a file**: Adds a new file to the directory.
- **Delete a file**: Removes a file from the directory.
- **List contents**: Shows the files and directories within the directory.
- **Rename**: Renames a file.
- **Traverse the file system**: Move between directories to locate files.

##### Example:
- Use the `mkdir()` system call to create a new directory:
```c
mkdir("newdir", 0755);
```

#### **System Call: `unlink()`**
- **Unlink** removes the directory entry for a file but does not immediately delete the file until all references are gone.

#### **System Call: `link()`**
- **Link** creates a new directory entry for an existing file, allowing multiple names (hard links) to refer to the same file.

---

### **Hard Links vs. Soft (Symbolic) Links**

#### **Hard Links**
- A **hard link** is a direct reference to the **inode** of a file. It creates another name for the same file.
- **Multiple hard links** share the same inode and data on disk. Changes to one hard link affect all others.
- Properties:
  - If one hard link is deleted, the data remains accessible via other hard links.
  - Hard links cannot span different file systems or directories.
  - Deleting the last hard link removes the actual file data.

##### Example:
```bash
ln original.txt hardlink.txt
```
This creates a hard link `hardlink.txt` pointing to the same inode as `original.txt`.

#### **Soft (Symbolic) Links**
- A **soft link** (or symbolic link) is a reference to the **pathname** of a file or directory.
- Properties:
  - Soft links can point to directories and cross file systems.
  - Deleting the original file leaves a **dangling link** (broken reference).
  - Soft links reference the file path, not the inode.

##### Example:
```bash
ln -s original.txt symlink.txt
```
This creates a soft link `symlink.txt` that points to `original.txt`.

---

### **Comparison of Hard Links and Soft Links**

| Feature                             | Hard Links                        | Soft (Symbolic) Links      |
| ----------------------------------- | --------------------------------- | -------------------------- |
| **Type of Reference**               | Direct reference to inode         | Reference to file path     |
| **Survives Original File Deletion** | Yes (until all links are deleted) | No (becomes a broken link) |
| **Supports Directories**            | No                                | Yes                        |
| **Cross-File System**               | No                                | Yes                        |
| **Same Inode**                      | Yes                               | No                         |
| **Creation Syntax**                 | `ln file link`                    | `ln -s file symlink`       |

---


### Part VI: **Making and Mounting a File System**

#### Making a File System
- **mkfs** (make filesystem): A tool to create a file system.
    - Input:
        - Device (e.g., `/dev/sda1`)
        - File system type (e.g., `ext3`)
    - It writes an empty file system starting with the root directory onto the disk partition.

#### Mounting a File System
- **Mounting**: The process of attaching a file system to the uniform file-system tree.
    - Use the **mount** command to paste the file system onto a directory tree.
        - Example: `mount -t ext4 /dev/sda1 /home/users`
    - **Unmounting**: Detaching a file system using the **umount** command.
        - Example: `umount /home/users`

#### Viewing Mounts
- To see mounted file systems: run the `mount` command without any arguments.

#### Long Options for Mount
- Use `-o` to specify options for mounting.
    - Example: `mount /dev/sda /mnt/home -o ro,uid=1000`
- Common options:
    - **exec, noexec**: Enable/disable execution of programs.
    - **suid, nosuid**: Enable/disable setuid programs.
    - **ro**: Mount in read-only mode.
    - **rw**: Mount in read-write mode.

#### `/etc/fstab`
- **fstab**: A configuration table for mounting/unmounting filesystems automatically during system boot.
    - Structure:
        - Device (path or UUID)
        - Mount point (use "none" for swap)
        - File system type
        - Options (use "defaults" for default options)
        - Backup operation (use `0`)
        - File system integrity check order (use `0` for no checks, `1` for root, `2` for others)
    - Use `blkid` to view device UUIDs.

#### Special Purpose File Systems
- **procfs**: Mounted at `/proc`, shows information about processes.
- **sysfs**: Mounted at `/sys`, exports kernel subsystem info.
- **tmpfs**: Mounted at `/run` and others, stores data in volatile memory (lost on reboot).

---
