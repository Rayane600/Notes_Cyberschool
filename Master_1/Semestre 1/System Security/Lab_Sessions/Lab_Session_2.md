# Task 1 
## *Exo 1*
```C
#include <stdio.h>
#include <stdlib.h>
#include <sys/wait.h>
#include <unistd.h>
int global_var = 100;
int main() {
	int local_var = 200;
	int *dynamic_var = malloc(sizeof(int));
	*dynamic_var = 300;
	printf("Before fork:\n");
	printf("Global: %d, Local: %d, Malloc: %d\n", global_var, local_var,
	       *dynamic_var);
	int pid = fork();
	int *dynamic_var_fork = malloc(sizeof(int));
	*dynamic_var_fork = 1000;
	if (pid == 0) {
		global_var = 500;
		local_var = 600;
		*dynamic_var = 700;
		printf("In child process:\n");
		printf("child process post fork variable adress %x \n",
		       dynamic_var_fork);
		printf("Global: %d, Local: %d, Malloc: %d\n", global_var,
		       local_var, *dynamic_var);
	} else {
		wait(NULL);
		global_var = 800;
		local_var = 900;
		*dynamic_var = 1000;
		printf("In parent process:\n");
		printf("parent process post fork variable adress %x \n",     dynamic_var_fork);
		printf("Global: %d, Local: %d, Malloc: %d\n", global_var,
		       local_var, *dynamic_var);
	}
	free(dynamic_var);
	free(dynamic_var_fork);
	return 0;
}
```
##### Note 
- even when calling malloc after forking the addresses are the same => both processes have different memory spaces.

## *Exo 2*

```C
#include <fcntl.h>
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/wait.h>

int main(int argc, char *argv[]) {
	int fd = open("output.txt", O_WRONLY | O_CREAT | O_TRUNC);
	if (fd < 0) {   
		printf("error opening")
        return 1;
    }
  pid_t pid = fork();

    if (pid == 0) {
        write(fd, "hello\n", 6);
    } else {
        wait(NULL);
        write(fd, "goodbye\n", 8);
    }

    close(fd);
    return 0;	
}

```
##### The reason the parent doesn't truncate the file and writes after is because the offset isn't resetted and is shared. If we want to read the file starting from the parent we have to seek(0)
## *Exo 3*

```C
#include <stdio.h>
#include <sys/wait.h>
#include <unistd.h>

int main() {
	pid_t pid = fork();

	if (pid == 0) {
		close(STDOUT_FILENO);
		printf("prout child.\n");
	} else {
		wait(NULL);
		printf("prout parent.\n");
	}

	return 0;
}

```

# Task 2
## Exo 1

```C
#include <stdio.h>
#include <string.h>
#include <unistd.h>
int main() {
	int pipe1[2];
	char buffer[4096] = {0};
	if (pipe(pipe1) == -1) {
		perror("pipe");
		return 1;
	}
	char *message = "Hello world!\n";
	write(pipe1[1], message, strlen(message));
	close(pipe1[1]);
	read(pipe1[0], buffer, sizeof(buffer));
	close(pipe1[0]);
	printf("Read : %s", buffer);
	return 0;
}
```

## Exo 2

```C
#include <stdio.h>
#include <string.h>
#include <sys/wait.h>
#include <unistd.h>
int main() {
	int pipefd[2];
	int pid;
	char buffer[4096] = {0};
	if (pipe(pipefd) == -1) {
		printf("pipe fail");
		return 1;
	}
	pid = fork();
	if (pid == -1) {
		printf("fork fail ");
		return 1;
	}
	if (pid == 0) {
		close(pipefd[1]);
		read(pipefd[0], buffer, sizeof(buffer));
		printf("Child read from pipe: %s", buffer);
	} else {
		close(pipefd[0]);
		char *message = "Message from parent to child\n";
		write(pipefd[1], message, strlen(message));
		wait(NULL);
	}
	return 0;
}
```
##### Note : Write end of the pipe is pipe\[1]. Read end of the pipe is pipe\[0]

# Task 3
```C
#include <stdio.h>
#include <stdlib.h>
#include <sys/wait.h>
#include <unistd.h>

int main(int argc, char *argv[]) {
	if (argc != 3) {
		printf("argument not valid");
		return 1;
	}
	int pipefd[2];
	if (pipe(pipefd) == -1) {
		printf("pipe error");
		return 1;
	}
	int pid1 = fork();
	if (pid1 == -1) {
		printf("fork error");
		return 1;
	}
	if (pid1 == 0) {
		close(pipefd[0]);
		dup2(pipefd[1], STDOUT_FILENO);
		execlp(argv[1], argv[1], NULL);
	} else {
		close(pipefd[1]);
		wait(NULL);
		dup2(pipefd[0], STDIN_FILENO);
		execlp(argv[2], argv[2], NULL);
	}
}
```

# Task 4
1. **List File Descriptors**
    
    - Open directory and print current file descriptors.
2. **Main Function**
    
    - **Step 1**: List initial file descriptors.
    - **Step 2**: Open files, list updated file descriptors.
    - **Step 3**: Create pipe, list updated file descriptors.
    - **Step 4**: Fork process.
        - Child: list file descriptors.
        - Parent: list file descriptors.
    - **Step 5**: Parent closes resources, list final state, wait for child.

## Task 5
### Recursive way
- **Recursive Function `piping(in, argtotal, argrest, argv[])`**
    
    - If last command:
        - Redirect input (`dup2`) and execute command (`execlp`).
    - Else:
        - Create a pipe.
        - Fork process.
        - **Child Process**:
            - Redirect input and output to pipe ends.
            - Execute command.
        - **Parent Process**:
            - Wait for child.
            - Recursively call `piping()` for the next command.
- **Main Function**
    
    - Call `piping()` with initial arguments to execute the chain of commands.
### Iterative way
**Main Function**
- Initialize input as `STDIN_FILENO`.
- Loop through each command:
    - If not the last command, create a pipe.
    - Fork a child process:
        - **Child Process**:
            - Redirect input (`dup2`) if needed.
            - Redirect output (`dup2`) to pipe if not the last command.
            - Execute command.
        - **Parent Process**:
            - Wait for child to finish.
            - Update input to read from the pipe for the next command.