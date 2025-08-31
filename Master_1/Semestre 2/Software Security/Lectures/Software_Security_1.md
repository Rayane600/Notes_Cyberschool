# Lecture 1/3

## Standardization and Evolution

C is an ISO standardized language with multiple versions:

- **C89** (ANSI C, widely disliked)
- **C99** (most commonly used in existing codebases)
- **C11** (modern features, widely supported)
- **C17** (minor revision of C11, almost identical)
- **C23** (latest revision with new improvements)

### GNU Extensions

- GCC provides extensions beyond the standard C language.
- Some GNU-specific code may not be compilable with other compilers (e.g., LLVM cannot compile the Linux kernel due to GNU extensions).

## Structure of a C Program

- **Source Files**: `.c` files (Translation Units)
- **Header Files**: `.h` files (contain function prototypes and macros)
- One `.c` file must contain the entry point (`main`).

## Phases of Compilation

The compiler processes a C program in multiple phases:

1. **Preprocessing** (Phase 4): Handles macros, includes, and conditional compilation.
2. **Compilation** (Phase 7): Translates preprocessed code into assembly.
3. **Linking** (Phase 8): Combines multiple object files and resolves dependencies.

## Preprocessor

The C preprocessor is a separate language that runs before actual compilation. The compiler does not see the preprocessor directives.

### Preprocessor Directives

Each directive starts with `#` and occupies one line:

- `#include` - Includes another file
- `#define` - Defines macros (when not defined it is initialized to 0)
- `#undef` - Undefines a macro
- `#if, #ifdef, #ifndef, #else, #elif, #endif` - Conditional compilation
- `#error` - Generates an error
- `#line` - Provides file name and line number information
- `#pragma` - Implementation-defined behavior

#### GCC Extensions:

- `#pragma once` (non-standard, replaces include guards)

## Memory Representation

Everything in memory is an array of bytes:

```c
int x = 4;
char *memory = (char*)&x;
short y = *memory;
```

Depending on the byte order (endianness), `y` might have different values.

- **Big-endian**: Most significant byte at the lowest address.
- **Little-endian**: Least significant byte at the lowest address. (the one used on our machines)

## Variables

- **lvalue**: Represents a memory location (modifiable, except for constants).
- **rvalue**: Represents a value (not a memory location).

### Examples:

- `&"hello"` is valid because a string literal is an lvalue.
- `&4` is invalid because a constant is not an lvalue.
- `(i++)++` is invalid because `(i++)` is an rvalue.
- `(*x) = 4` is valid because `*x` is an lvalue.
- Functions return rvalues

### Compound Literals

- Temporary and unnamed objects stored in memory (usually within a function scope).
- Example:
    
    ```c
    int *arr = (int []){1,2,3,4};
    ```
    
    **Difference**:
    
    ```c
    int arr[] = {1,2,3,4};  // Static array
    int *arr = (int []){1,2,3,4}; // Pointer to temporary array
    ```
    

## Scope and Lifetime of Objects

### Scope Types

- **Block Scope**: Visible within a specific block (e.g., inside a function).
- **File Scope**: Visible throughout the translation unit.
- **Function Scope**: Visible within a single function (e.g., labels).
- **Function Prototype Scope**: Applies to function parameter declarations.

### Storage Duration

- **Automatic**: Exists only within the block where it is declared.
- **Static**: Exists for the lifetime of the program.
- **Allocated**: Exists until manually deallocated (e.g., `malloc`).
- **Thread-local**: Exists for the duration of a thread.

## Linkage

- **No Linkage**: Cannot be referred to outside its scope.
- **Internal Linkage**: Accessible within the same translation unit (`static` variables at file scope).
- **External Linkage**: Accessible across multiple translation units (`extern` variables and functions).

### Example of Undefined Behavior

```c
int x = 42;
int main() {
    int x = x; // Undefined behavior: local x shadows global x
}
```

## Objects and Alignment

- Objects are contiguous sequences of bytes.
- **Alignment**: Objects may have alignment constraints (determined by `_Alignof`).
- Padding may be added to structures to meet alignment requirements.

## Function Declarations

- Must return a valid type (cannot be an array type).
- **Declaration vs Definition**:
    - `int func(int);` // Declaration
    - `int func(int x) { return x + 1; }` // Definition

### `_Noreturn`

- Indicates a function never returns (e.g., `exit()` or `abort()`).

### `inline`

- Suggests the compiler to optimize function calls.
- Example:
    
    ```c
    static inline int square(int x) { return x * x; }
    ```
    

## Arrays

- Can be declared with a known or unknown size.
- Variable Length Arrays (VLAs) are allocated dynamically within a function.
- Example of initialization:
    
    ```c
    int y[200] = { [99] = 5 }; // Sets y[99] to 5, others default to 0
    ```
    

## Pointers and Void Pointers

- `void *` pointers store addresses but cannot be dereferenced directly.
- Example:
    
    ```c
    void *ptr;
    int x = 5;
    ptr = &x;
    ```
    
## Namespaces in C

C organizes identifiers into **four separate namespaces**, allowing different types of identifiers to share the same name without conflict:

1. **Label Namespace** – Contains `goto` labels (unique within a function).
2. **Tag Namespace** – Contains names of `struct`, `union`, and `enum` types.
3. **Member Namespace** – Each `struct` or `union` has its own namespace for member names.
4. **Ordinary Identifiers** – Includes variable names, function names, `typedef` names, and enumeration constants.

### Namespace Lookup
- The compiler determines an identifier's namespace based on its usage.
- Different namespaces allow the same name to be used in multiple contexts without conflict.

### Using `typedef` to Inject Tag Names
`typedef` can bring `struct` and `union` names into the ordinary identifier namespace:
```c
typedef struct Point { int x, y; } Point;
Point p1;  // Now "Point" is a valid type without needing "struct"

## Definitions vs Declarations

- **Declaration**: Introduces an identifier without allocating memory.
- **Definition**: Allocates memory (unless `extern`).

Example:

```c
extern int x;  // Declaration (no memory allocated)
int x = 10;    // Definition (memory allocated)
```
