# Lecture 2/3

---

## Kinds of Compiled Files
- **Relocatable Object Files (.o)**:
  - Contain code and data in a form that can be combined with other relocatable files.
  - Produced from a single source `.c` file.
- **Executable Object Files (.out / a.out)**:
  - These are assembler outputs that are copied into memory and executed.
- **Shared Object Files (.so)**:
  - Files that can be loaded into memory and linked dynamically at either **load time** or **run-time**.
  - **Load time linking:** Use `-l` with `gcc`.
  - **Run-time linking:** Use `dlopen` and `dlsym`.

---

##  Executable and Linkable Format (ELF)
- **One unified format** for:
  - Relocatable object files (`.o`)
  - Executable binaries (`a.out`)
  - Shared object files (`.so`)
- **Two separate views of an executable:**
  1. **Linking View**:
     - Used at **static link time** to combine relocatable files.
     - **Section headers** are only useful at this stage; they become **useless after linking**.
  2. **Loading View**:
     - Used at **run time** to load and execute programs.
     - **Program headers** define how the OS loads the program into memory.
     - **For object files, program headers are useless.**

- If we **remove the section header table**, we can't use **GDB** for debugging anymore because GDB relies on parsing this information.
- **The program needs the dynamic symbol table to run.** 
  - To remove the symbol table, use: `strip a.out`.

---

##  ELF File Structure

### ELF Header
- Identifies the file as an ELF binary.
- Contains:
  - Magic number (`7F 45 4C 46` → `0x7F 'E' 'L' 'F'`).
  - Machine architecture (`EM_X86_64`, `EM_ARM`, etc.).
  - Entry point address (`e_entry` → where execution starts).
  - Offsets for program and section headers (`e_phoff`, `e_shoff`).

### Sections vs Segments
- **Sections** (before linking, found in `.o` files):
  - Contain raw data and metadata.
  - Used for **compilation** but disappear at runtime.
- **Segments** (after linking, found in executables):
  - Sections are **grouped into segments** by the linker.
  - Segments dictate **how the OS loads sections into memory**.

###  Important ELF Sections
- **.text** → Code (executable instructions).
- **.rodata** → Read-only data (constants, strings).
- **.data** → Initialized variables (writable).
- **.bss** → Uninitialized variables (zero-filled at runtime).
- **.init & .fini** → Initialization & finalization code.
- **.symtab & .strtab** → Symbol table & associated strings (removed in stripped binaries).
- **.dynsym & .dynstr** → Dynamic linking symbols & strings.

---

##  Program Headers & Segments
- **Program Headers define executable segments**:
  - **PT_LOAD** → Loadable segment (code or data).
  - **PT_DYNAMIC** → Dynamic linking info.
  - **PT_INTERP** → Path to ELF interpreter (e.g., `/lib64/ld-linux-x86-64.so.2`).

- **Flags for runtime access permissions:**
  - `PF_X` → Executable.
  - `PF_W` → Writable.
  - `PF_R` → Readable.

---

##  Security Considerations
- **Stripping Binaries (`strip a.out`)**
  - Removes symbol tables to make reverse engineering harder.
  - `strip` removes `.symtab` and `.strtab`, but not `.dynsym`.
- **ELF Injection & Malware**
  - Modifying ELF segments can lead to **code injection**.
  - Overwriting `.init_array` can execute malicious code at startup.
- **Stack Canaries & NX Bit**
  - **Stack canaries** protect against buffer overflows.
  - **NX Bit (No Execute)** prevents executing writable memory.

---

##   Takeaways
- ELF binaries have **two views**: **Linking (compile-time)** & **Loading (run-time)**.
- Removing section headers prevents **GDB debugging**.
- Removing **dynamic symbol tables** (`strip`) reduces information leakage but doesn’t prevent execution.
- **Shared object files (.so)** allow both **load-time and run-time linking**.
