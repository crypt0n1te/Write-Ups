# pearl pearl pearl

The challenge is on a service at `95.216.233.106:26551`

connecting to the service shows:

```
$ nc 95.216.233.106 26551
ctf{pearlpearlpearl}
ctf{pearlpearlpearl}
ctf{pearlpearlpearl}
ctf{pearlpearlpearl}
ctf{pearlpearlpearl}
ctf{pearlpearlpearl}
ctf{pearlpearlpearl}
ctf{pearlpearlpearl}
ctf{pearlpearlpearl}
ctf{pearlpearlpearl}
ctf{pearlpearlpearl}
ctf{pearlpearlpearl}
ctf{pearlpearlpearl}
ctf{pearlpearlpearl}
...
```

The flag for this challenge was hidden in the new line characters

I then extracted the new line characters using python and converted the binary characters to the flag

```
import socket

hostname = '95.216.233.106'
port = 18058

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect((hostname, port))

totalData = ''

while True:
    data = s.recv(100000)
    if len(data) > 0:
        totalData += str(data).replace('ctf{pearlpearlpearl}','').replace('pearl','')
    else:
        break

Binary = ''
count = 0
for char in totalData:
    if char == 'r':
        Binary += '0'
        count += 1
    if char == 'n':
        Binary += '1'
        count += 1
    if count == 8:
        Binary += ' '
        count = 0

flag = Binary.split()

for binary in flag:
    print(chr(int(binary,2)), end='')

print()
s.close()

```

ractf{p34r1_1ns1d3_4_cl4m}


