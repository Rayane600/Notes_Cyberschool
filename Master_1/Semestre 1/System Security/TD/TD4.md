# True false
## Q1.
False, Only the inode number has to be identical the name can be different.
## Q2.
True since soft links refer to path names.
## Q3.
Each call of open will create a new entry in the OFT since sharing the same entry would cause offset problems for reading.
## Q4.
False. It will only increment the reference count by one for all open files.
## Q5.
No since it would create loops in referencing.
## Q6.
False some `/tmp` files are saved. It is not the same as tmpfs which is actually volatile.

---
# File Systems.
## Q7.
```bash
#!/bin/bash
ls -la /proc/$$/fd | cut -d" " -f12
```
## Q8.
- A new file descriptor is created for the new file.
- A new inode is allocated for the file. And the inode bitmap is updated
- Directory entry creation The directory structure is updated to include the new file.
- New entry to the OFT is created.
- Create an entry in the per-process file table.
## Q9.
1. **Directory Entry**: A new directory entry is added to the target directory, pointing to the existing inode of the file.
2. **Inode**: The reference count of the inode is incremented to reflect that it is now referenced by multiple directory entries.

## Q10.

When a file descriptor is duplicated using the `dup` system call:
1. **File Descriptor Table**: A new entry is created in the file descriptor table, pointing to the same file table entry.
2. **Open File Table**: The open file table entry remains the same, but its reference count is incremented.

## Q11.

An empty directory has at  two links: one from its parent and one from the `.` entry within the directory itself.

## Q12.

```bash
Inode=$(stat -c %i filename)
Device=$(stat -c %d filename)
sudo find / -inum $Inode -exec stat -c '%n %d' {} \; | awk -v dev="$Device" '$2 == dev {print $1}'

```

## Q13.

$$
(n \times 2^b )+ (2^{b-a} \times 2^b) +(2^{(b-a)²} \times 2^b)
$$
