
---
## Scopes and Qualifiers
- **Scopes** define where variables are accessible in the code.
- **Qualifiers** modify how variables behave or how the compiler treats them.

### Variable Declaration Modifiers:
- `auto` (default): Local variables.
- `static`: Variables maintain their value across function calls.
- `extern`: Declares variables defined elsewhere.
- `const`: Variables cannot be modified after initialization.
- `volatile`: Prevents compiler optimizations, for variables modified externally.

### Example:
```c
void tick() {
    static int cpt = 0;
    cpt++;
    printf("Counter = %d\n", cpt);
}
```

---

## Memory Model and Allocation Classes
- **Memory Areas:**
  - Code: `.text`
  - Data: `.data`, `.bss`
  - Heap: Dynamically allocated data.
  - Stack: Local data.
  
### Variable Types:
- **Global Variables:** Declared outside functions; stored in the `.bss` section.
- **Local Variables:** Declared inside blocks; stored in the stack.

---

## Pointers
- Pointers store the address of variables or functions.

### Example:
```c
int main() {
    int x = 7;
    int *ptr = &x;
    printf("Value of x = %d\n", *ptr); // Outputs 7
}
```

### Pointer Arithmetic:
```c
int arr[5] = {1, 2, 3, 4, 5};
int *ptr = &arr[2];
ptr++; // Points to arr[3]
```

---

## Dynamic Memory Allocation
- Use `malloc()`, `calloc()`, `realloc()` for memory allocation.
- Always check if `malloc()` returns `NULL` and free allocated memory with `free()`.

### Example:
```c
int *arr = malloc(5 * sizeof(int));
if (arr == NULL) {
    // handle allocation failure
}
free(arr);
```

---

## Parameter Passing in C
- Default: **By value**, meaning a copy of the variable is passed.
- To modify a variable from a function, pass its pointer (by reference).

### Example:
```c
void increment(int *x) {
    (*x)++;
}
int main() {
    int a = 5;
    increment(&a);
    printf("%d\n", a); // Outputs 6
}
```

---

## Implicit and Explicit Type Conversion
- **Implicit Conversion:** Automatic type promotion during operations.
- **Explicit Conversion (Casting):** Change type manually using `(type)`.

### Example:
```c
float f = 3.9;
int x = (int) f; // x is 3, truncating 0.9
```

---

## Standard C Library (libc)
- Provides essential operations for I/O, memory management, and string manipulation.
  
### Key Functions:
- **Memory:** `malloc()`, `free()`, `memcpy()`, `memset()`
	- The `malloc` function allocates a block of memory of a specified size and returns a pointer to the beginning of the block. The contents of the allocated memory are **uninitialized** (i.e., the memory may contain garbage values). ` void* malloc(size_t size);
	- The `calloc` function allocates memory for an array of elements and initializes the memory to zero. It differs from `malloc` in that it clears (zeroes out) the memory and takes two arguments: the number of elements and the size of each element. `void* calloc(size_t num, size_t size);`
	- The `realloc` function changes the size of an already allocated memory block. If the new size is larger, `realloc` may allocate a new block, copy the old data, and free the old block. If the new size is smaller, it might shrink the block without reallocating.`void* realloc(void* ptr, size_t new_size);`
	
- **String:** `strcpy()`, `strcmp()`, `strlen()`
- **I/O:** `fopen()`, `fread()`, `fwrite()`, `printf()`, `scanf()`
	