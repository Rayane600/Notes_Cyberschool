chip sur la clé : 24C02R8  K129K
- Quand la clé est branchée a l'horizontale (face chip vers l'intérieur) minicom le reconnait comme une clé alors qu'a l'inverse non.


## Bus Pirate I2c
100 MHz
```
I2C>(1)  
Searching I2C address space. Found devices at:  
0xAE(0x57 W) 0xAF(0x57 R) 0xBE(0x5F W) 0xBF(0x5F R)
I2C>(2)  
Sniffer  
Any key to exit  
(press the button on the card)
[0xA0-[][0xA2-[][0xA4-[][0xA6-[][0xA8-[][0xAA-[][0xAC-[][0xAE+0x00-0xAF+0x25+0xAE+][0xAF]
```

## Dumping the firmware of the card 
- use openocd ->`openocd -f /usr/share/openocd/scripts/interface/stlink.cfg -f /usr/share/openocd/scripts/target/stm32l1x_dual_bank.cfg` -> setup a listener `telnet localhost 4444` then dump the assembly code.
- We used ghidra to help navigate the assembly which was usefull to get the function call tree to find the main function (which was also branched into by the reset_handeler) 
- this is the code of the main function we found : 
```C
void FUN_00000484(void)

{
  char cVar1;
  char cVar2;
  int local_10;
  char local_9;
  
  FUN_00000ad0();
  FUN_000004f4();
  FUN_00000688();
  FUN_000007cc();
  local_9 = '\x01';
  do {
    cVar1 = FUN_0000078c();
    if ((cVar1 == '\0') && (local_9 != '\0')) {
      cVar2 = FUN_00000a3c();
      if (cVar2 == '\0') {
        FUN_00000ac2();
      }
      else if (cVar2 == '\x01') {
        FUN_00000ab8();
      }
    }
    for (local_10 = 0; local_9 = cVar1, local_10 < 10000; local_10 = local_10 + 1) {
    }
  } while( true );
}

```
we thought the first four functions were just environment setup and didn't actually need to be studied we tried to trace the functions inside the do-while.

Mapping of certain fucntions

|Function|Likely Role|
|---|---|
|`FUN_00000718()`|Turn LED **on**|
|`FUN_00000750(n)`|Blink LED **n** times|
|`FUN_00000ab8()`|**Wrong code** → LED on + halt|
|`FUN_00000ac2()`|**Correct code** → Blink 5 times|
## Bonne clé 
premier entier : 0x40