## Task.1:
1. `dd if=/dev/zero of=task1.img bs=1M count=100`
2. `mkfs.ext4 task1.img`
3. 
```BASH
mkdir mount
sudo mount -o loop task1.img mount
```
4. a- `sudo touch hello.txt` in the mounted dir then write stuff.
   b- `sudo umount mount`
   c- `strings task1.img` task1.img is a binary file that contains the data of the unmounted filesystem
5. No we can't create new files even in sudo
6. No we cannot execute scripts or binarys since it is in no exec
## Task.2:
1. added this line to the fstab `/home/rayane/Cyberschool/LabSessions/SystemSecurity/Lab2/task1.img /home/rayane/Cyberschool/LabSessions/SystemSecurity/Lab2/mount ext4 loop,defaults 0 0`
2. modified the line to 
   `/home/rayane/Cyberschool/LabSessions/SystemSecurity/Lab2/task1.img /home/rayane/Cyberschool/LabSessions/SystemSecurity/Lab2/mount ext4 loop,user,defaults 0 0
`
## Task.3:
```bash
sudo mount -t binfmt_misc none /proc/sys/fs/binfmt_misc
sudo nvim /etc/binfmt.d/python3.conf
```
add this line `:python3:E::py::/usr/bin/python3:`
## Task.4:

