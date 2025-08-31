## Task 1
- When setting the interpreter to something that isn't an elf file it crashes.
- When using a C file that calls the right interpreter through execve it also crashes through a segfault 

## Task 2
```zsh
gcc -shared -o libTask2.so encrypt.c

gcc -o main main.c  -L. -lTask2 -Wl,-rpath=.

ldd ./main  
       linux-vdso.so.1 (0x000072bae21b1000)  
       libTask2.so => ./libTask2.so (0x000072bae21a1000)  
       libc.so.6 => /usr/lib/libc.so.6 (0x000072bae1f6c000)  
       /lib64/ld-linux-x86-64.so.2 => /usr/lib64/ld-linux-x86-64.so.2 (0x000072bae21b3000)
readelf -s main | grep encrypt  
    3: 0000000000000000     0 FUNC    GLOBAL DEFAULT  UND encrypt  
   11: 0000000000000000     0 FUNC    GLOBAL DEFAULT  UND encrypt

LD_DEBUG=symbols ./main 2>&1 | grep encrypt
```
