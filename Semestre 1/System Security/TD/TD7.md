
# Unix DAC
## Q1.
- a) Read/Write for user and group, no rights for others:  
    Octal: `660`  
    Representation: `rw-rw----`
- b) Read/Execute for user, Read for group and others:  
    Octal: `544`  
    Representation: `r-xr--r--`
- c) Read/Write/Execute for user, Read/Execute for group, and Read for others:  
    Octal: `754`  
    Representation: `rwxr-xr--`

## Q2.
You need to be the owner or the root, no further permissions are needed. We need X permissions for the parent directory. We don't need the write permissions because we're only modifying the inode of the file (the inode is in the directory but only it's identifier (an integer))

Directory entry => Map <string, pointer>
                      name,14  ---> SUPER BLOCK(Bitmaps , inodes , blocks)

## Q3.
Only the  root of the file can change the owner. The `chown` command requires root privileges.
Why : 
1. SUID : avoid privilege escalation.
2. Auditing : it makes tracing back problems possible
We can `chown` the group if we are the owner of the file.
## Q4
The root can assign group management by adding a user to a group and enabling them to use the `gpasswd` or `usermod` command. By setting a group admin
## Q5 
The root user can read and write any file regardless of its permissions.

# Unix Permissions
## Q6
1. No, permission denied
2. No, permission denied
3. Yes we can 

## Q7
Mohamed can only read in notes.txt
## Q8
1. If mohamed is root => changes groups
2. else if the group has a password => permission denied
3. else if the group doesn't have a password => changes group
## Q9
Nothing,
we could change the group and then try to read with the group permissions, but we only check the owner permissions since mohamed is the owner.

## Q10
Yes he can because even though professors isn't his primary groups there is no distinction made between primary and supplementary.

## Q11
