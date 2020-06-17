# Really Powerful Gnomes
This challenge wasn't really anything special, just automate the process of fighting bosses until you have enough money to get a new weapon, rinse and repeat until you get the "tank" which can defeat the gnomes, when you do that the flag is printed.

```python
from pwn import *

p = remote('jh2i.com', 50031)
initial = p.clean(1).decode("UTF-8")

p.sendline("6")
p.sendline("1")
new = p.clean(1).decode("utf-8")

for i in range(0, 2500):
  p.sendline("5")

p.sendline("6")
p.sendline("4")
next = p.clean(1).decode("utf-8")
print(next)

for i in range(0, 3000):
  p.sendline("2")

p.interactive()
```