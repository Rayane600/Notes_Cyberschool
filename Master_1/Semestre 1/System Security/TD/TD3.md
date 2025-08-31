#  True/False
## Q1 :
Yes it could and the two processes would just have a different PID.
Any process can run any program without locking the program.
## Q2 :
No each process has it's own. A child process owns but a copy of the parent process'.
# QCM
## Q3 : 
`fork-exec-wait`
fork -> exec the command sleep -> wait for the child to terminate
## Q4 : 
- The contents of the file descriptor table are the same for the child and it's parent 
## Q5 :
a- P runs immediately 
b- the sys call blocks P
c- P runs immediately 
## Q6 :
- PID doens't change 
- `/proc/fd` doesn't change
- The value of the program counter stored within the user space context on the kernel stack (PC Register). It is reinitialized.
#### System() syscall
- Will create a subshell to run the commands. So the PID changes, the  `/proc/fd` doesn't change (at least at the beginning of the instruction) and the PC register is reinitialized.
## Q7 :
- two direct children of P are created
- One other descendent of P is created 
# Forks go mad
## Q8 :
One of the possible outputs could be 
```

```
##### do again
## Q9 :

`count in child 0`

## Q10 :
`PPid/status`
Name : Bash
pid : 10
state : waiting/sleeping
## Q11 : 

```C
#include <stdio.h>
#include <stdlib.h>
#include <sys/wait.h>
#include <unistd.h>s
int main(int argc, char *argv[]) {
	fd = open();
	fork();
	if(child){
		stdout = fd;
		exec(arg[1]);
	}
	if (parent){
		wait();
		stdin = fd;
		seek(0);
		exec(arg[2]);
		}
}
```