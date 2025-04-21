
## Task 1 

```zsh
~/Documents/Cyberschool/LabSessions/SoftwareSecurity/Lab8 readelf -s Task1.so    
  
Symbol table '.dynsym' contains 10 entries:  
  Num:    Value          Size Type    Bind   Vis      Ndx Name  
    0: 0000000000000000     0 NOTYPE  LOCAL  DEFAULT  UND    
    1: 0000000000000000     0 NOTYPE  WEAK   DEFAULT  UND _ITM_deregisterT[...]  
    2: 0000000000000000     0 NOTYPE  WEAK   DEFAULT  UND __gmon_start__  
    3: 0000000000000000     0 NOTYPE  WEAK   DEFAULT  UND _ITM_registerTMC[...]  
    4: 0000000000000000     0 FUNC    WEAK   DEFAULT  UND [...]@GLIBC_2.2.5 (2)  
    5: 0000000000001109    11 FUNC    GLOBAL DEFAULT   12 foo  
    6: 0000000000004018     8 OBJECT  GLOBAL DEFAULT   22 c  
    7: 0000000000002000     4 OBJECT  GLOBAL DEFAULT   14 a  
    8: 0000000000001114    27 FUNC    GLOBAL DEFAULT   12 main  
    9: 0000000000002004     4 OBJECT  GLOBAL DEFAULT   14 b  
  
Symbol table '.symtab' contains 20 entries:  
  Num:    Value          Size Type    Bind   Vis      Ndx Name  
    0: 0000000000000000     0 NOTYPE  LOCAL  DEFAULT  UND    
    1: 0000000000000000     0 FILE    LOCAL  DEFAULT  ABS Task1.c  
    2: 0000000000004010     4 OBJECT  LOCAL  DEFAULT   22 d  
    3: 0000000000000000     0 FILE    LOCAL  DEFAULT  ABS    
    4: 0000000000001130     0 FUNC    LOCAL  DEFAULT   13 _fini  
    5: 0000000000004008     0 OBJECT  LOCAL  DEFAULT   22 __dso_handle  
    6: 0000000000003e08     0 OBJECT  LOCAL  DEFAULT   19 _DYNAMIC  
    7: 0000000000002010     0 NOTYPE  LOCAL  DEFAULT   15 __GNU_EH_FRAME_HDR  
    8: 0000000000004020     0 OBJECT  LOCAL  DEFAULT   22 __TMC_END__  
    9: 0000000000003fe8     0 OBJECT  LOCAL  DEFAULT   21 _GLOBAL_OFFSET_TABLE_  
   10: 0000000000001000     0 FUNC    LOCAL  DEFAULT   10 _init  
   11: 0000000000000000     0 NOTYPE  WEAK   DEFAULT  UND _ITM_deregisterT[...]  
   12: 0000000000002004     4 OBJECT  GLOBAL DEFAULT   14 b  
   13: 0000000000000000     0 NOTYPE  WEAK   DEFAULT  UND __gmon_start__  
   14: 0000000000001109    11 FUNC    GLOBAL DEFAULT   12 foo  
   15: 0000000000004018     8 OBJECT  GLOBAL DEFAULT   22 c  
   16: 0000000000002000     4 OBJECT  GLOBAL DEFAULT   14 a  
   17: 0000000000001114    27 FUNC    GLOBAL DEFAULT   12 main  
   18: 0000000000000000     0 NOTYPE  WEAK   DEFAULT  UND _ITM_registerTMC[...]  
   19: 0000000000000000     0 FUNC    WEAK   DEFAULT  UND __cxa_finalize@G[...]  
~/Documents/Cyberschool/LabSessions/SoftwareSecurity/Lab8 readelf -s a.out                   
  
Symbol table '.dynsym' contains 6 entries:  
  Num:    Value          Size Type    Bind   Vis      Ndx Name  
    0: 0000000000000000     0 NOTYPE  LOCAL  DEFAULT  UND    
    1: 0000000000000000     0 FUNC    GLOBAL DEFAULT  UND _[...]@GLIBC_2.34 (2)  
    2: 0000000000000000     0 NOTYPE  WEAK   DEFAULT  UND _ITM_deregisterT[...]  
    3: 0000000000000000     0 NOTYPE  WEAK   DEFAULT  UND __gmon_start__  
    4: 0000000000000000     0 NOTYPE  WEAK   DEFAULT  UND _ITM_registerTMC[...]  
    5: 0000000000000000     0 FUNC    WEAK   DEFAULT  UND [...]@GLIBC_2.2.5 (3)  
  
Symbol table '.symtab' contains 28 entries:  
  Num:    Value          Size Type    Bind   Vis      Ndx Name  
    0: 0000000000000000     0 NOTYPE  LOCAL  DEFAULT  UND    
    1: 0000000000000000     0 FILE    LOCAL  DEFAULT  ABS Task1.c  
    2: 0000000000004010     4 OBJECT  LOCAL  DEFAULT   22 d  
    3: 0000000000000000     0 FILE    LOCAL  DEFAULT  ABS    
    4: 0000000000003e20     0 OBJECT  LOCAL  DEFAULT   19 _DYNAMIC  
    5: 0000000000002014     0 NOTYPE  LOCAL  DEFAULT   14 __GNU_EH_FRAME_HDR  
    6: 0000000000003fe8     0 OBJECT  LOCAL  DEFAULT   21 _GLOBAL_OFFSET_TABLE_  
    7: 0000000000000000     0 FUNC    GLOBAL DEFAULT  UND __libc_start_mai[...]  
    8: 0000000000000000     0 NOTYPE  WEAK   DEFAULT  UND _ITM_deregisterT[...]  
    9: 0000000000004000     0 NOTYPE  WEAK   DEFAULT   22 data_start  
   10: 0000000000002008     4 OBJECT  GLOBAL DEFAULT   13 b  
   11: 0000000000004020     0 NOTYPE  GLOBAL DEFAULT   22 _edata  
   12: 0000000000001140     0 FUNC    GLOBAL HIDDEN    12 _fini  
   13: 0000000000004000     0 NOTYPE  GLOBAL DEFAULT   22 __data_start  
   14: 0000000000000000     0 NOTYPE  WEAK   DEFAULT  UND __gmon_start__  
   15: 0000000000004008     0 OBJECT  GLOBAL HIDDEN    22 __dso_handle  
   16: 0000000000002000     4 OBJECT  GLOBAL DEFAULT   13 _IO_stdin_used  
   17: 0000000000001119    11 FUNC    GLOBAL DEFAULT   11 foo  
   18: 0000000000004028     0 NOTYPE  GLOBAL DEFAULT   23 _end  
   19: 0000000000001020    38 FUNC    GLOBAL DEFAULT   11 _start  
   20: 0000000000004018     8 OBJECT  GLOBAL DEFAULT   22 c  
   21: 0000000000002004     4 OBJECT  GLOBAL DEFAULT   13 a  
   22: 0000000000004020     0 NOTYPE  GLOBAL DEFAULT   23 __bss_start  
   23: 0000000000001124    27 FUNC    GLOBAL DEFAULT   11 main  
   24: 0000000000004020     0 OBJECT  GLOBAL HIDDEN    22 __TMC_END__  
   25: 0000000000000000     0 NOTYPE  WEAK   DEFAULT  UND _ITM_registerTMC[...]  
   26: 0000000000000000     0 FUNC    WEAK   DEFAULT  UND __cxa_finalize@G[...]  
   27: 0000000000001000     0 FUNC    GLOBAL HIDDEN    10 _init
   ```

 After stripping:
```zsh
~/Documents/Cyberschool/LabSessions/SoftwareSecurity/Lab8 readelf -s a.out    
  
Symbol table '.dynsym' contains 6 entries:  
  Num:    Value          Size Type    Bind   Vis      Ndx Name  
    0: 0000000000000000     0 NOTYPE  LOCAL  DEFAULT  UND    
    1: 0000000000000000     0 FUNC    GLOBAL DEFAULT  UND _[...]@GLIBC_2.34 (2)  
    2: 0000000000000000     0 NOTYPE  WEAK   DEFAULT  UND _ITM_deregisterT[...]  
    3: 0000000000000000     0 NOTYPE  WEAK   DEFAULT  UND __gmon_start__  
    4: 0000000000000000     0 NOTYPE  WEAK   DEFAULT  UND _ITM_registerTMC[...]  
    5: 0000000000000000     0 FUNC    WEAK   DEFAULT  UND [...]@GLIBC_2.2.5 (3)  
~/Documents/Cyberschool/LabSessions/SoftwareSecurity/Lab8 readelf -s Task1.so    
  
Symbol table '.dynsym' contains 10 entries:  
  Num:    Value          Size Type    Bind   Vis      Ndx Name  
    0: 0000000000000000     0 NOTYPE  LOCAL  DEFAULT  UND    
    1: 0000000000000000     0 NOTYPE  WEAK   DEFAULT  UND _ITM_deregisterT[...]  
    2: 0000000000000000     0 NOTYPE  WEAK   DEFAULT  UND __gmon_start__  
    3: 0000000000000000     0 NOTYPE  WEAK   DEFAULT  UND _ITM_registerTMC[...]  
    4: 0000000000
    
    000000     0 FUNC    WEAK   DEFAULT  UND [...]@GLIBC_2.2.5 (2)  
    5: 0000000000001109    11 FUNC    GLOBAL DEFAULT   12 foo  
    6: 0000000000004018     8 OBJECT  GLOBAL DEFAULT   22 c  
    7: 0000000000002000     4 OBJECT  GLOBAL DEFAULT   14 a  
    8: 0000000000001114    27 FUNC    GLOBAL DEFAULT   12 main  
    9: 0000000000002004     4 OBJECT  GLOBAL DEFAULT   14 b

```

the symtable is removed and only the dynamic symbol table remains.

## Task 2

```zsh
~/Documents/Cyberschool/LabSessions/SoftwareSecurity/Lab8 gcc -shared -o Task2.so Task2.c -fPIC -fvisibil  
ity=hidden  
~/Documents/Cyberschool/LabSessions/SoftwareSecurity/Lab8 gcc -shared -o Task2l.so Task2.c -Wl,--strip-al  
l  
~/Documents/Cyberschool/LabSessions/SoftwareSecurity/Lab8 nm Task2.so    
                w __cxa_finalize@GLIBC_2.2.5  
0000000000004000 d __dso_handle  
0000000000003e48 d _DYNAMIC  
0000000000001100 t _fini  
00000000000010e9 t foo  
0000000000003fe8 d _GLOBAL_OFFSET_TABLE_  
                w __gmon_start__  
0000000000002000 r __GNU_EH_FRAME_HDR  
0000000000001000 t _init  
                w _ITM_deregisterTMCloneTable  
                w _ITM_registerTMCloneTable  
0000000000004008 d __TMC_END__  
~/Documents/Cyberschool/LabSessions/SoftwareSecurity/Lab8 nm Task2l.so  
nm: Task2l.so: no symbols
```

Compiling with -fvisibility=hidden will change the visibility of global symbols in the dynamic symbol table 

*/!\ when using readelf -s on a file Ndx = UND means that the sybol has external linkage*

## Task 3 
