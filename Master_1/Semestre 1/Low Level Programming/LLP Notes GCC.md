Here is a list of common GCC (GNU Compiler Collection) options and what they do:

### Compilation Options:
1. **`-c`**: Compiles source files without linking. Produces object files (.o).
   - Example: `gcc -c file.c`
2. **`-S`**: Compiles source files to assembly code without linking.
   - Example: `gcc -S file.c`
3. **`-E`**: Runs only the preprocessor and outputs the result.
   - Example: `gcc -E file.c`
4. **`-o <file>`**: Specifies the output file name.
   - Example: `gcc -o output file.c`
5. **`-g`**: Includes debugging information in the output (useful for debugging tools like `gdb`).
   - Example: `gcc -g file.c`
6. **`-O`**: Enables optimization. This can take levels such as `-O`, `-O1`, `-O2`, `-O3`, `-Ofast`, and `-Os`.
   - Example: `gcc -O2 file.c`
   - `-Os`: Optimize for size.
   - `-Ofast`: Aggressive optimizations, ignoring strict standards.
7. **`-Wall`**: Enables most warning messages.
   - Example: `gcc -Wall file.c`
8. **`-Wextra`**: Enables additional warning messages not covered by `-Wall`.
   - Example: `gcc -Wextra file.c`
9. **`-Werror`**: Treats all warnings as errors.
   - Example: `gcc -Werror file.c`
10. **`-pedantic`**: Enforces strict ISO C and ISO C++ compliance, issuing warnings for any non-standard features.
    - Example: `gcc -pedantic file.c`
11. **`-std=<standard>`**: Specifies the language standard (e.g., `c90`, `c99`, `c11`, `gnu99`, `gnu11`).
    - Example: `gcc -std=c99 file.c`
12. **`-D<macro>`**: Defines a preprocessor macro.
    - Example: `gcc -DDEBUG file.c`
13. **`-I<dir>`**: Adds a directory to the list of directories to search for header files.
    - Example: `gcc -I./includes file.c`
14. **`-L<dir>`**: Adds a directory to the list of directories to search for libraries.
    - Example: `gcc -L./lib file.c`
15. **`-l<library>`**: Links with a specific library.
    - Example: `gcc -lm file.c` (links with the math library)

### Linking Options:
16. **`-shared`**: Generates a shared library (e.g., `.so` files on Linux).
    - Example: `gcc -shared -o libfoo.so foo.c`
17. **`-static`**: Forces linking with static libraries instead of shared ones.
    - Example: `gcc -static file.c`
18. **`-rdynamic`**: Adds all symbols to the dynamic symbol table, useful for debugging shared libraries.
    - Example: `gcc -rdynamic file.c`

### Optimization Options:
19. **`-funroll-loops`**: Unrolls loops for optimization purposes.
    - Example: `gcc -O2 -funroll-loops file.c`
20. **`-fomit-frame-pointer`**: Omits the frame pointer in functions, often used for optimization.
    - Example: `gcc -O2 -fomit-frame-pointer file.c`
21. **`-march=<architecture>`**: Generates code optimized for a specific processor architecture (e.g., `x86-64`, `armv7`).
    - Example: `gcc -march=x86-64 file.c`
22. **`-mtune=<processor>`**: Optimizes the code for a specific processor but still supports generic processors.
    - Example: `gcc -mtune=generic file.c`
23. **`-ffast-math`**: Enables faster, less precise math operations.
    - Example: `gcc -ffast-math file.c`

### Debugging and Profiling:
24. **`-p`**: Enables profiling with `gprof`.
    - Example: `gcc -p file.c`
25. **`-pg`**: Enables profiling with `gprof`, but also includes extra information for function call analysis.
    - Example: `gcc -pg file.c`
26. **`-fsanitize=<type>`**: Enables runtime error detection with sanitizers (e.g., `address`, `thread`, `undefined`).
    - Example: `gcc -fsanitize=address file.c`

### Miscellaneous Options:
27. **`-v`**: Enables verbose mode, showing detailed compilation steps.
    - Example: `gcc -v file.c`
28. **`--version`**: Displays the GCC version.
    - Example: `gcc --version`
29. **`-pipe`**: Uses pipes rather than temporary files for intermediate steps in the compilation process.
    - Example: `gcc -pipe file.c`
30. **`-fPIC`**: Generates position-independent code, typically for shared libraries.
    - Example: `gcc -fPIC file.c`
31. **`-fopenmp`**: Enables OpenMP support for parallel programming.
    - Example: `gcc -fopenmp file.c`
32. **`-pthread`**: Links with the POSIX thread library.
    - Example: `gcc -pthread file.c`
33. **`-M`**: Generates a dependency list for `make` (without compiling).
    - Example: `gcc -M file.c`

### Preprocessor Options:
34. **`-include <file>`**: Automatically includes a file before compiling the source code.
    - Example: `gcc -include config.h file.c`
35. **`-P`**: Suppresses line number information in the preprocessor output.
    - Example: `gcc -E -P file.c`

This list includes many of the most common GCC options, but there are many more depending on specific use cases, architectures, and platforms. You can always refer to the GCC manual (`man gcc`) or use `gcc --help` to explore additional options.

---
```C

void init_grid(grid_t* grid, int size, FILE* output)
{
    bool solvable = false;
#ifdef DEBUG
    int max_attempts = 10;
#else
    int max_attempts = 10000000;
#endif
    int attempt_count = 0;
    srand(time(NULL));
    grid->size = size;
    grid_allocate(grid, size);

    // Character set: '1' to '9' and 'A' to 'F'
    char characters[] = {'1', '2', '3', '4', '5', '6', '7', '8',
                         '9', 'a', 'b', 'c', 'd', 'e', 'f'};

    while (!solvable && attempt_count < max_attempts)
    {
        attempt_count++;
        printf("Attempt %d: Generating grid...\n", attempt_count);

        // Step 1: Set up an initial patterned grid to avoid consecutive
        // duplicates
        for (int i = 0; i < grid->size; i++)
        {
            for (int j = 0; j < grid->size; j++)
            {
                // Cycle through characters to fill the grid without immediate
                // duplicates
                char base_value =
                    characters[(i + j) % (size < 16 ? size : 16)];
                set_value(i, j, grid, base_value);
                set_color(i, j, grid, Grey);
            }
        }

        // Step 2: Randomize a subset of cells to add variation
        for (int k = 0; k < size; k++)
        {
            int  row        = rand() % grid->size;
            int  col        = rand() % grid->size;
            char random_val = characters[rand() % (size < 16 ? size : 16)];
            set_value(row, col, grid, random_val);
        }

        // Step 3: Apply heuristics before solving for an early validity check
        if (!apply_heuristics(grid, 0))
        {
            printf("Grid failed preliminary heuristics check, retrying...\n");
            continue;
        }

        // Step 4: Attempt solving with the heuristic-checked grid
        grid_t* g = grid_solver(grid, MODE_FIRST, 0, output, NULL, 0);
        if (g != NULL)
        {
            printf("Solvable grid found.\n");
            solvable = true;
            grid_print(grid, output);
        }
        else
        {
            printf("Generated grid is not solvable, retrying...\n");
        }
    }

    if (!solvable)
    {
        fprintf(stderr,
                "Failed to generate a solvable grid after %d attempts.\n",
                max_attempts);
    }
}
```