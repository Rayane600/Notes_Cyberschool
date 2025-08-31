# Building a Shellcode

3. This program creates the file `/tmp/pwn` if it doesn't exist already otherwise opens it. then calls the syscall exit.
after removing null bytes and doing the jump-call-pop trick :
```
.text
.globl _start
_start:
jmp filename
_pop:
pop %ebx
push $2
pop %ecx
push $8
pop %eax
int $0x80
push $42
pop %ebx
push $1
pop %eax
int $0x80
filename:
call _pop
.string "/tmp/pwn"
```
1. `char shellcode[] = "\xeb\x11\x5b\x6a\x02\x59\x6a\x08\x58\xcd\x80\x6a\x2a\x5b\x6a\x01\x58\xcd\x80\xe8\xea\xff\xff\xff\x2f\x74\x6d\x70\x2f\x70\x77\x6e";`
2. 