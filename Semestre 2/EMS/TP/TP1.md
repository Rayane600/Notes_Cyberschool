## Question 1:
types of memory:
- EEPROM 16Kb
- SRAM 80Kb
- Flash 512Kb

## Question 2:
The different Sections:
- `isr_vector` stocked in FLASH
- `text` stocked in FLASH
- `rodata` stocked in FLASH
- `ARM.extab` stocked in FLASH
- `ARM` stocked in FLASH
- `preinit_array` stocked in FLASH
- `init_array` stocked in FLASH
- `fini_array` stocked in FLASH
- `data` stocked in RAM at FLASH
- `bss` stocked in RAM
- `_user_heap_stack` stocked RAM
## Question3:
```C
/* Copy the data segment initializers from flash to SRAM */

ldr r0, =_sdata /* start address for the initialization values of the .data section */

ldr r1, =_edata /* end address for the .data section */

ldr r2, =_sidata /* start address for the .data section */

movs r3, #0 /* loop counter */

b LoopCopyDataInit

  

CopyDataInit:

ldr r4, [r2, r3] /* load data from flash */

str r4, [r0, r3] /* store data to SRAM */

adds r3, r3, #4 /* increment counter */

  

LoopCopyDataInit:

adds r4, r0, r3 /* calculate end address */

cmp r4, r1 /* compare with end address */

bcc CopyDataInit /* branch if < */
```
## Question 4:
the entry point is `RESET_HANDLER`

We add `Entry(Reset_Handler)` line at the start of the linker script 

## Question 5:
```C
MEMORY

{

FLASH (rx) : ORIGIN = 0x08000000, LENGTH = 512K

RAM (xrw) : ORIGIN = 0x20000000, LENGTH = 80K

}
```

## Question 6:
`arm-none-eabi-readelf -s blinky.elf`

  165: 20000028     4 OBJECT  GLOBAL DEFAULT    8 blinkCounter
## Question 7:


## Question 8:

`08000f24 <Reset_Handler>:`

## Question 9:
```C
08000378 <delay>:

8000378: 2100 movs r1, #0

800037a: e006 b.n 800038a <delay+0x12>

800037c: bf00 nop

800037e: 3301 adds r3, #1

8000380: f241 3287 movw r2, #4999 @ 0x1387

8000384: 4293 cmp r3, r2

8000386: ddf9 ble.n 800037c <delay+0x4>

8000388: 3101 adds r1, #1

800038a: 4288 cmp r0, r1

800038c: dd01 ble.n 8000392 <delay+0x1a>

800038e: 2300 movs r3, #0

8000390: e7f6 b.n 8000380 <delay+0x8>

8000392: 4770 bx lr
```

une double boucle pour mettre un delai

## Question 10: 
Affiche tous les registres et leur contenu (seulement quand on est en `halt`).

## Question 11:
Selon le manuel de programmation quand le bit 0 (TPL) est a 1 on est en mode non privilégié => on voit sa valeur grace à `reg`

## Question 12:
`program /home/rayane/Downloads/TP/TP1/blinky/build/blinky.elf`