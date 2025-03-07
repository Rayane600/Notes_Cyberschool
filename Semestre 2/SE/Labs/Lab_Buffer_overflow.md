  
## Rewriting the saved eip

for i386, we have : 

```
(gdb) info frame  
Stack level 0, frame at 0xffffcbc4:  
eip = 0x56556193 in foo (rewrite_saved_eip-1.c:5); saved eip = 0x565561e4  
called by frame at 0xffffcc00  
source language c.  
Arglist at 0xffffcbbc, args: a=1, b=97 'a', c=2.5  
Locals at 0xffffcbbc, Previous frame's sp is 0xffffcbc4  
Saved registers:  
 ebp at 0xffffcbbc, eip at 0xffffcbc0  
(gdb) p /x &buffer  
$1 = 0xffffcbb0
(gdb) p /x 0xffffcbc0 - 0xffffcbb0
$1 = 0x10
```
So we have to modify : 
`ret = (int *)(buffer + 0);`
by 
`ret = (int *)(buffer + 16);`

Now, when we do `(gdb) disas main` : 

```
.....
0x565561df <+53>:    call   0x5655617d <foo>
0x565561e4 <+58>:    add    $0xc,%esp
0x565561e7 <+61>:    movl   $0x1,-0xc(%ebp)
0x565561ee <+68>:    sub    $0x8,%esp
0x565561f1 <+71>:    push   -0xc(%ebp)
0x565561f4 <+74>:    lea    -0x1230(%ebx),%eax
0x565561fa <+80>:    push   %eax
0x565561fb <+81>:    call   0x56556040 <printf@plt>
.....
```
so to jump from +61 to +71, we need to do : 
`(*ret) += 10;`


For amd64, we have : 

```
(gdb) info frame  
Stack level 0, frame at 0x7fffffffd9b0:  
rip = 0x55555555514a in foo (rewrite_saved_eip-1.c:5); saved rip = 0x555555555194  
called by frame at 0x7fffffffd9d0  
source language c.  
Arglist at 0x7fffffffd9a0, args: a=1, b=97 'a', c=2.5  
Locals at 0x7fffffffd9a0, Previous frame's sp is 0x7fffffffd9b0  
Saved registers:  
 rbp at 0x7fffffffd9a0, rip at 0x7fffffffd9a8  
(gdb) p /x &buffer  
$1 = 0x7fffffffd990  
(gdb) p /x 0x7fffffffd9a8 - 0x7fffffffd990  
$2 = 0x18
```
so we modify `ret = (int *)(buffer + 0);` by `ret = (int *)(buffer + 24);`

then we do `(gdb) disas main`
```
.....
0x000055555555518f <+35>:    call   0x555555555139 <foo>  
0x0000555555555194 <+40>:    movl   $0x1,-0x4(%rbp)  
0x000055555555519b <+47>:    mov    -0x4(%rbp),%eax  
0x000055555555519e <+50>:    mov    %eax,%esi  
0x00005555555551a0 <+52>:    lea    0xe5d(%rip),%rax
.....
```
and we find we have to jump 7 bytes.
`(*ret) += 7;

for i386, we have : 
```
(gdb) info frame  
Stack level 0, frame at 0xffffc9d0:  
eip = 0x5655618d in foo (rewrite_saved_eip-2.c:5); saved eip = 0x565561eb  
called by frame at 0xffffca00  
source language c.  
Arglist at 0xffffc9c8, args:    
Locals at 0xffffc9c8, Previous frame's sp is 0xffffc9d0  
Saved registers:  
 ebp at 0xffffc9c8, eip at 0xffffc9cc  
(gdb) p /x &buffer  
$1 = 0xffffc9a4  
(gdb) p /d &buffer  
$2 = -13916  
(gdb) p /x &buffer  
$3 = 0xffffc9a4  
(gdb) p /x 0xffffc9cc - 0xffffc9a4  
$4 = 0x28  
(gdb) p /d 0xffffc9cc - 0xffffc9a4  
$5 = 40
```
since it is an int buffer we do ` ret = buffer + 10;` since it is 40 bytes and an int is 4 bytes
```
  0x565561e9 <+45>:    call   0x5655617d <foo>  
  0x565561ee <+50>:    addl   $0x1,-0xc(%ebp)  
  0x565561f2 <+54>:    cmpl   $0xe,-0xc(%ebp)  
  0x565561f6 <+58>:    jle    0x565561e9 <main+45>  
  0x565561f8 <+60>:    movl   $0x1,-0xc(%ebp)  
  0x565561ff <+67>:    sub    $0x8,%esp
```
so we do `  (*ret) += 17;`

now to change i to 10 
```
(gdb) info frame  
Stack level 0, frame at 0xffffca00:  
eip = 0x565561d9 in main (rewrite_saved_eip-2.c:13); saved eip = 0xf7d6b357  
source language c.  
Arglist at 0xffffc9e8, args:    
Locals at 0xffffc9e8, Previous frame's sp is 0xffffca00  
Saved registers:  
 ebx at 0xffffc9e4, ebp at 0xffffc9e8, eip at 0xffffc9fc  
(gdb) p /d 0xffffc9c8 - 0xffffc9a4  
$1 = 36  
(gdb) p /x &buffer  
$2 = 0xf7f7ae28  
(gdb) p /x 0xf7f7ae28 - 0xffffca00  
$3 = 0xf7f7e428  
(gdb) p /d 0xf7f7ae28 - 0xffffca00  
$4 = -134749144  
(gdb) p /x &i  
$5 = 0xffffc9dc  
(gdb) b foo  
Breakpoint 2 at 0x5655618d: file rewrite_saved_eip-2.c, line 5.  
(gdb) c  
Continuing.  
  
Breakpoint 2, foo () at rewrite_saved_eip-2.c:5  
5         ret = buffer + 10;  
(gdb) p /x &buffer  
$6 = 0xffffc9a4  
(gdb) p /x 0xffffc9a4 - 0xffffc9dc  
$7 = 0xffffffc8  
(gdb) p /d 0xffffc9a4 - 0xffffc9dc  
$8 = -56  
(gdb) p /d  0xffffc9dc - 0xffffc9a4  
$9 = 56
```
56 bytes is 14 int so we do `ret = buffer + 14;`

For amd64, we have : 
```
#include <stdio.h>
int foo(void) {
  int buffer[8];
  int *ret;
  // ret = buffer + 10;
  ret = buffer + 14;
  (*ret) += 17;
  // ret = buffer + 14;
  ret = buffer + 19;
  (*ret) = 10;
  return 0;
}

int main(void) {
  int i = 0;
  for (i = 0; i < 15; i++) foo();
  i = 1;
  printf("%d\n", i);
  return 0;
}
```