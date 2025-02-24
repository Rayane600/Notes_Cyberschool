
## Low Level Programming (LLP) - Tools Overview

### Compilation Toolchain
- **Preprocessor**: Handles `#define`, `#include`, and macros.
- **Compiler**: Translates C code into object code (.o).
- **Assembler**: Converts assembly code into machine code.
- **Linker**: Combines object files into a binary executable.
  
Use `gcc` to perform all these steps in one go:  
`gcc -o output source.c`

### GCC (GNU Compiler Collection)
- **Released**: 1987
- **Supported Languages**: C, C++, Fortran, Java, Ada, Objective-C, etc.
- **Portable**: Runs on most platforms and supports various processors.
- **Competitor**: `clang` is a popular alternative.
  
Common gcc flags:
- `-O<level>`: Optimization levels (0 to 3, s for space optimization).
- `-Wall -Wextra`: Enable warnings.
- `-std=c11`: Set the C standard to C11.

### Preprocessor Basics
- **Directives**: All start with `#` (e.g., `#include`, `#define`).
- **Macros**:
  - Simple macros: `#define PI 3.14`
  - Parameterized macros: `#define max(x, y) ((x) > (y) ? (x) : (y))`
  - **Best Practice**: Enclose macro arguments in parentheses to avoid errors (e.g., `#define MULT(a, b) ((a)*(b))`).

### Modular Programming in C
- Split the code into **modules** with separate header and source files.
  - **Header File (.h)**: Contains constants, type definitions, function declarations.
  - **Source File (.c)**: Contains the actual code of the functions.
  
Example:
```c
// Header file (gridio.h)
#ifndef GRIDIO_H
#define GRIDIO_H

#define MAXSIZE 10
typedef struct {
  int size;
  char *content;
} t_grid;

extern void write_to_file(FILE *f, t_grid *g);
extern void read_from_file(FILE *f, t_grid *g);

#endif
```

### Linking
- **Static Linking**: Links all object files into the executable during compilation.
- **Dynamic Linking**: The binary depends on external shared libraries (`.so`) at runtime.

Command examples:
- Static linking: `gcc -o project main.o libgrid.a`
- Dynamic linking: `gcc -o project -L. main.o -lgridio`

### Make and Makefiles
- **Make**: A build automation tool that helps build projects by automating compilation steps.
- **Makefile Structure**:
  ```makefile
  target: dependencies
  	commands to build target
  ```
  
Example:
```makefile
project: main.o gridio.o
	gcc -o project main.o gridio.o

main.o: main.c main.h gridio.h
	gcc -I./ -c main.c

gridio.o: gridio.c gridio.h
	gcc -I./ -c gridio.c
```

- **Common Makefile Targets**:
  - `all`: Builds the entire project.
  - `clean`: Deletes all build artifacts (`.o` files, executables).
  - `help`: Displays help text.
  
Variables and Automatic Variables:
- Variables: `CC=gcc`, `CFLAGS=-std=c11 -Wall`
- Automatic variables: `$@` (target), `$<` (first dependency), `$^` (all dependencies).

### Good Practices
- Always compile with warnings (`-Wall -Wextra`).
- Separate code into modules (`foo.h`, `foo.c` for each module).
- Use Makefiles, even for small projects.
- Use `DEBUG` and `VERBOSE` flags for conditional compilation.


A Makefile is a special file used by the `make` build automation tool to compile and link programs efficiently. It defines the set of tasks to be executed, such as compiling C source files and linking them into an executable. Here's a detailed breakdown of a typical Makefile structure for a C project:

### Basic Structure

1. **Targets**: These are the goals the `make` command aims to achieve. Common targets include building an executable, cleaning up object files, or compiling specific source files.

2. **Rules**: A rule defines how to make a target. It has the following form:

   ```make
   target: prerequisites
       command
   ```

   - **Target**: The file or action you want to generate or execute.
   - **Prerequisites (dependencies)**: Files that the target depends on. If any of these files are modified, the target is considered out-of-date and must be rebuilt.
   - **Command**: The command(s) executed to build the target. These commands must be indented with a **tab**, not spaces.

3. **Variables**: These are placeholders to simplify the Makefile. You can define variables for compiler flags, directories, and file lists.

4. **Implicit Rules**: Make has built-in rules for common file extensions. For example, it knows how to compile `.c` files into `.o` object files without explicitly defining rules for each file.

5. **Phony Targets**: These are not files but symbolic names for operations like `clean`, `install`, etc. Declaring them as `.PHONY` ensures that `make` doesn’t treat them as actual file targets.

---

### Detailed Breakdown

Let's go through each part with a practical example. Assume you have the following project structure:

```
.
├── src/
│   ├── main.c
│   ├── file1.c
│   └── file2.c
└── include/
    └── myheader.h
```

#### 1. Variables

Variables make the Makefile more modular and easier to maintain:

```make
# Compiler
CC = gcc

# Compiler flags
CFLAGS = -Wall -Wextra -Iinclude

# Directories
SRCDIR = src
INCDIR = include
OBJDIR = obj
BINDIR = bin

# Output binary
BINARY = $(BINDIR)/myprogram

# Source files
SRC = $(wildcard $(SRCDIR)/*.c)

# Object files
OBJ = $(SRC:$(SRCDIR)/%.c=$(OBJDIR)/%.o)
```

- `CC`: Specifies the compiler to use. Here, `gcc` is the GNU C Compiler.
- `CFLAGS`: Flags passed to the compiler. `-Wall` enables all warnings, and `-Iinclude` tells the compiler to look for header files in the `include` directory.
- `SRCDIR`, `INCDIR`, `OBJDIR`, `BINDIR`: Directories for source files, headers, object files, and the final binary.
- `SRC`: This uses `wildcard` to find all `.c` files in `src/`.
- `OBJ`: A pattern replacement that generates object file paths corresponding to the `.c` files in `SRC`. For example, `src/main.c` becomes `obj/main.o`.

#### 2. Targets and Rules

##### Build the Binary

```make
$(BINARY): $(OBJ)
	@mkdir -p $(BINDIR)
	$(CC) $(OBJ) -o $(BINARY)
```

- **Target**: `$(BINARY)` is the output executable.
- **Prerequisites**: It depends on the object files in `$(OBJ)`.
- **Command**: 
  - `@mkdir -p $(BINDIR)` ensures the binary directory exists.
  - `$(CC) $(OBJ) -o $(BINARY)` links the object files into the final executable.

##### Compile Object Files

```make
$(OBJDIR)/%.o: $(SRCDIR)/%.c
	@mkdir -p $(OBJDIR)
	$(CC) $(CFLAGS) -c $< -o $@
```

- **Target**: This pattern rule compiles `.c` files into `.o` files.
- **Prerequisites**: It depends on the corresponding `.c` file.
- **Command**:
  - `@mkdir -p $(OBJDIR)` ensures the object directory exists.
  - `$(CC) $(CFLAGS) -c $< -o $@` compiles the source file (`$<`) into the object file (`$@`).

##### Clean Up

```make
.PHONY: clean
clean:
	rm -rf $(OBJDIR) $(BINDIR)
```

- **.PHONY**: Declares `clean` as a phony target, so `make` knows it's not a file.
- **Command**: `rm -rf $(OBJDIR) $(BINDIR)` removes the object and binary directories.

##### Install (Optional)

```make
.PHONY: install
install: $(BINARY)
	@cp $(BINARY) /usr/local/bin
```

- **Target**: `install` copies the compiled binary to `/usr/local/bin` for system-wide usage.

#### 3. Dependencies

When compiling, object files typically depend on header files. You can specify these dependencies to ensure that changes to headers trigger recompilation:

```make
$(OBJDIR)/main.o: $(SRCDIR)/main.c $(INCDIR)/myheader.h
$(OBJDIR)/file1.o: $(SRCDIR)/file1.c $(INCDIR)/myheader.h
$(OBJDIR)/file2.o: $(SRCDIR)/file2.c $(INCDIR)/myheader.h
```

This ensures that if `myheader.h` is modified, the corresponding `.c` files are recompiled.

#### 4. Complete Example

```make
# Variables
CC = gcc
CFLAGS = -Wall -Wextra -Iinclude
SRCDIR = src
INCDIR = include
OBJDIR = obj
BINDIR = bin
BINARY = $(BINDIR)/myprogram
SRC = $(wildcard $(SRCDIR)/*.c)
OBJ = $(SRC:$(SRCDIR)/%.c=$(OBJDIR)/%.o)

# Targets
$(BINARY): $(OBJ)
	@mkdir -p $(BINDIR)
	$(CC) $(OBJ) -o $(BINARY)

$(OBJDIR)/%.o: $(SRCDIR)/%.c
	@mkdir -p $(OBJDIR)
	$(CC) $(CFLAGS) -c $< -o $@

.PHONY: clean
clean:
	rm -rf $(OBJDIR) $(BINDIR)

.PHONY: install
install: $(BINARY)
	@cp $(BINARY) /usr/local/bin
```

### Makefile Workflow
1. **Run `make`**: By default, it builds the first target (in this case, `$(BINARY)`), which compiles the source files into object files and then links them into the final executable.
2. **Run `make clean`**: Removes the binary and object files.
3. **Run `make install`**: Copies the binary to `/usr/local/bin`.

### Advanced Features
- **Automatic Dependency Generation**: Tools like `gcc` or `clang` can generate dependencies automatically for you.
  
  ```make
  -include $(OBJ:.o=.d)
  
  $(OBJDIR)/%.d: $(SRCDIR)/%.c
      @mkdir -p $(OBJDIR)
      $(CC) $(CFLAGS) -MMD -c $< -o $(OBJDIR)/$*.o
  ```

This generates `.d` files containing header dependencies and ensures that changes to header files trigger recompilation.

This is a detailed guide to writing a Makefile for a C project, providing efficiency, modularity, and ease of use during development.

Example Hitori's makefile : 
```make
# Variables
SRCDIR=src
INCDIR=include
BINARY=hitori
CFLAGS=-I$(INCDIR) -Wall -Wextra -std=c99 -g

# Source files
SRCS=$(SRCDIR)/hitori.c $(SRCDIR)/grid.c $(SRCDIR)/grid_io.c  $(SRCDIR)/heuristics.c
OBJS=$(SRCS:.c=.o)

# Targets
all: $(BINARY)

$(BINARY): $(OBJS)
	@echo "Linking $(BINARY)..."
	@$(CC) $(OBJS) -o $(BINARY)
	@echo "Build complete!"

$(SRCDIR)/%.o: $(SRCDIR)/%.c
	@echo "Compiling $<..."
	@$(CC) $(CFLAGS) -c $< -o $@

clean:
	@echo "Cleaning up..."
	@rm -f $(SRCDIR)/*.o
	@rm -f $(BINARY)
	@echo "Clean complete!"

help:
	@echo "Targets in this Makefile:"
	@echo "  all     - Build the hitori binary."
	@echo "  clean   - Remove the binary and object files."
	@echo "  debug   - Build with debugging enabled."
	@echo "  help    - Display this help message."

# Debug target
debug: CFLAGS += -DDEBUG
debug: $(BINARY)

# Phony targets
.PHONY: all clean help debug

```
