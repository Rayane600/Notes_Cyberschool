| ELF Header Field | Content                                                         | Use                                                                 |
| ---------------- | --------------------------------------------------------------- | ------------------------------------------------------------------- |
| `e_ident`        | Magic number, class (32-bit or 64-bit), endianness, ABI, etc.   | Identifies the file as ELF and provides basic metadata.             |
| `e_type`         | Object file type (relocatable, executable, shared object, core) | Defines how the ELF file should be interpreted.                     |
| `e_machine`      | Target architecture (e.g., x86, x86-64, ARM)                    | Specifies the CPU architecture for execution.                       |
| `e_version`      | ELF version (usually 1)                                         | Ensures compatibility with the ELF format version.                  |
| `e_entry`        | Entry point virtual address                                     | Defines where execution starts for executables.                     |
| `e_phoff`        | Program header table offset                                     | Points to the program header table for segment loading information. |
| `e_shoff`        | Section header table offset                                     | Points to the section header table for linking and debugging.       |
| `e_flags`        | Architecture-specific flags                                     | Contains processor-specific flags for execution.                    |
| `e_ehsize`       | ELF header size                                                 | Specifies the size of the ELF header.                               |
| `e_phentsize`    | Size of each program header entry                               | Defines the size of a single program header entry.                  |
| `e_phnum`        | Number of program header entries                                | Specifies the count of program headers.                             |
| `e_shentsize`    | Size of each section header entry                               | Defines the size of a single section header entry.                  |
| `e_shnum`        | Number of section header entries                                | Specifies the count of section headers.                             |
| `e_shstrndx`     | Section header string table index                               | Index of the section containing section names.                      |

### Program Header Table (Segments)

|Field|Content|Use|
|---|---|---|
|`p_type`|Segment type (LOAD, DYNAMIC, INTERP, NOTE, etc.)|Defines how a segment is treated by the loader.|
|`p_offset`|Offset of the segment in the file|Specifies where the segment data is stored in the ELF file.|
|`p_vaddr`|Virtual address in memory|Defines where the segment should be mapped in memory.|
|`p_paddr`|Physical address (used in embedded systems)|Relevant for physical memory mapping in certain environments.|
|`p_filesz`|Size of the segment in the file|Specifies how much space the segment occupies in the file.|
|`p_memsz`|Size of the segment in memory|Defines how much memory should be allocated for the segment.|
|`p_flags`|Permissions (R, W, X)|Defines the access rights of the segment in memory.|
|`p_align`|Alignment constraint|Specifies how the segment must be aligned in memory.|

### Section Header Table (Sections)

|Field|Content|Use|
|---|---|---|
|`sh_name`|Name of the section (index in .shstrtab)|Defines the section’s name.|
|`sh_type`|Type of section (PROGBITS, NOBITS, STRTAB, etc.)|Defines how the section is used.|
|`sh_flags`|Flags (ALLOC, EXEC, WRITE)|Specifies memory attributes.|
|`sh_addr`|Virtual memory address (if loaded)|Defines where the section is mapped in memory.|
|`sh_offset`|Offset in the file|Specifies where the section is located in the ELF file.|
|`sh_size`|Size of the section|Defines how much data the section holds.|
|`sh_link`|Link to another section (e.g., symbol table reference)|Used for linking and debugging purposes.|
|`sh_info`|Additional section-specific information|Provides extra details based on section type.|
|`sh_addralign`|Alignment requirement|Ensures correct memory alignment.|
|`sh_entsize`|Entry size (for tables like symbol tables)|Specifies the size of individual entries within the section.|

### Common ELF Sections

|Section|Content|Use|
|---|---|---|
|`.text`|Executable machine code|Stores the program’s instructions.|
|`.data`|Initialized global/static variables|Stores writable variables initialized in the source code.|
|`.bss`|Uninitialized global/static variables|Allocates space for variables that start uninitialized.|
|`.rodata`|Read-only data (strings, constants)|Stores constants and string literals.|
|`.symtab`|Symbol table|Stores function and variable names for linking/debugging.|
|`.strtab`|String table for symbol names|Stores names for symbols in `.symtab`.|
|`.shstrtab`|Section name string table|Stores section names.|
|`.rel.text`|Relocation information for `.text`|Contains relocation entries for position-dependent code.|
|`.rel.data`|Relocation information for `.data`|Contains relocation entries for data segments.|
|`.dynamic`|Dynamic linking information|Stores shared library linking data.|
|`.dynsym`|Dynamic symbol table|Stores symbols used in dynamic linking.|
|`.dynstr`|Dynamic string table|Stores strings for `.dynsym`.|
|`.got`|Global Offset Table|Assists in dynamic symbol resolution.|
|`.plt`|Procedure Linkage Table|Handles function calls to dynamically linked libraries.|
|`.debug`|Debugging information|Contains debugging symbols (DWARF, etc.).|
|`.note`|Metadata notes|Stores additional metadata about the binary.|
|`.init`|Initialization code|Runs before `main()` in some executables.|
|`.fini`|Cleanup code|Runs after `main()` or on exit.|

