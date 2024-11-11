
---

# C Basics

## Data Types

### Integer Types (from `<stdint.h>`)
- **char**: 1 byte (may represent a character or a small integer)
- **short int**: 2 bytes
- **int**: system-dependent (typically 4 bytes)
- **long int**: at least as large as `int`
- **long long int**: larger than `long int`

### Type Qualifiers
- **signed**: allows negative and positive values (default)
- **unsigned**: only positive values

#### Notes:
- Size of types may vary between systems.
- Use `sizeof(type)` to get the size (in bytes) of any type or variable.
- For consistent sizes across platforms, use the fixed-width types from `<stdint.h>`.

### Integer Types from `<stdint.h>`
- Use when exact control over the size of integers is required.
- **Signed Types**: `int8_t`, `int16_t`, `int32_t`, `int64_t`
- **Unsigned Types**: `uint8_t`, `uint16_t`, `uint32_t`, `uint64_t`
- **Pointer-Sized Type**: `uintptr_t` (used for pointer arithmetic)

### Floating Point Types
- **float**: single-precision (typically 4 bytes)
- **double**: double-precision (typically 8 bytes)
- **long double**: extended precision (implementation-dependent)

### Boolean Type (from `<stdbool.h>` in C99)
- **Type**: `bool`
- **Values**: `true` (1) or `false` (0)

### `void` Type
- Represents the absence of a type (used in functions with no return or parameters).
- `sizeof(void)` is undefined, but `sizeof(void*)` gives the size of a pointer.

## Constants

### Integer Constants
- **Decimal**: `77`, `-44`
- **Octal**: Prefix `0` (e.g., `044`)
- **Hexadecimal**: Prefix `0x` (e.g., `0xff`)
- **No binary literals** (binary literals are not supported in C, but can be done using bitwise operations)

### Floating Point Constants
- Examples: `-38.44`, `-0.35e7` (scientific notation)

### Character Constants
- Characters are represented as integers (`'A'` is equivalent to `65`).
- Escape sequences: `\n` (newline), `\0` (null), `\t` (tab), etc.

### Boolean Constants (C99)
- `true`, `false`

### Strings
- String literals are enclosed in double quotes: `"hello"`
- Strings in C are arrays of `char` terminated by the null character (`'\0'`).

## Enumerations (enums)
- Defines a set of named integer constants.
```c
enum colors { RED, GREEN, BLUE };
```
- Values are assigned sequentially starting from `0` unless specified otherwise:
```c
enum colors { RED = 3, GREEN, BLUE };
```

## Structures (structs)
- Group different types of data into a single unit.
```c
struct mystruct {
    int x;
    char y;
    float z;
};
```
- Usage: `struct mystruct s; s.x = 5;`

#### Note: 
Memory padding may occur to align data members for performance.

## Unions
- A union stores multiple fields in the same memory space (overlapping fields).
```c
union myunion {
    int x;
    float y;
};
```
- Usage: `union myunion u; u.x = 5;`

#### Note:
- You can only store and use one field at a time; the last written value will determine the type of the data stored.

## Arrays
- Declaration:
```c
float array1[5][6];  // 2D array
int array2[5] = { 10, 32, 34, 41, 35 };  // 1D array with initialization
```
- Access elements: `int i = array2[2];`

#### Notes:
- Arrays are zero-indexed.
- C does not check array bounds at runtime.

## Strings (as char arrays)
- **No native string type** in C; strings are arrays of `char` terminated by `'\0'`.
```c
char str[] = "hello";  // Equivalent to {'h', 'e', 'l', 'l', 'o', '\0'}
```
- Common string manipulation functions are found in `<string.h>`: `strcpy()`, `strcat()`, `strlen()`.

#### Notes:
- Strings are a common source of bugs (e.g., buffer overflows).

## Typedef
- Defines a new name for an existing type.
```c
typedef struct { int x; float y; } mystruct_t;
typedef enum { RED, GREEN, BLUE } color_t;
typedef uintptr_t address_t;
```
- Now, `mystruct_t` can be used as a type alias for the unnamed `struct`.

## Variable Declaration and Initialization
- Variables must be declared before use.
- Syntax: `<type> <variable_list>;`
- Example:
```c
int i, j;
short a = 0, c = 4;
uint32_t b = 44;
```

## Functions

### Function Declaration
```c
return_type function_name(type1 param1, type2 param2) {
    // Function body
    return value;
}
```
- If a function has no parameters or return value, use `void`.
- Return types can be scalars, structures, or pointers, but **not arrays**.

### Function Prototypes
- Declared before the function definition or in a header file.
```c
void print_message(char *message);
```

#### Notes:
- **C passes arguments by value**, meaning the function receives a copy of the variable.
- **No function nesting** in standard C (though some compilers like GCC support it).

--- 
### Conditional 
```C
if (c =='\n){
	eol = true;
}
	else {
	nb_car++;
}
```
### Ternary operator
- More compact if-then-else
```C
max = (va>vb) ? va : vb;
```
- `<cond> ? <v1>:<v2>`
### Switch-case
```C
switch (v) {
	case 1: v=5; break;
	case 2: v=6; // fallthrough
	case 3: v=77; break;
	default: exit(0);
}
```
### While loop
```C
int nbcar = 0;
while (str[i] != ‘\0’) {
	nbcar++;
}
```
### for loop
```C
// Compute sum of elements in tab
for (int i=0;i<N;i++) {
	sum += tab[i];
}
```
### Do-while loop 
```C
do {
	printf("Entrer un chiffre: ")
	c = getchar();
} while (c < '0' && c > '9’);
```
### Useful loop statements
- **Break** -> exit loop body.
- **Continue** -> skip end of loop body.
- **goto \<label>** -> Branch to a given label in code.
### Special sequences in format string
- `%d` integer, decimal display
- `%x` integer, hexa display
- `%ld` long integer
- `%c` char
- `%s` string
- `%f`float
- `%c` character
