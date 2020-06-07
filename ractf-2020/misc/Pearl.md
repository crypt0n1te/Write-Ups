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

I then extracted the new line characters using python

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
        print(str(data).replace('ctf{pearlpearlpearl}',''))
    else:
        break
s.close()
```

```
$ python3 pearl.py
b'\r\n\n\n\r\r\n\r\r\n\n\r\r\r\r\n\r\n\n\r\r\r\n\n\r\n\n\n\r\n\r\r\r\n\n\r\r\n\n\r\r\n\n\n\n\r\n\n\r\n\n\n\r\r\r\r\r\r\n\n\r\r\n\n\r\r\n\n'
b'\r\n\r\r\r\n\n\n\r\r\n\r\r\r\n\n\r\r\r\n\r\n\r\n\n\n\n\n\r\r\n\n\r\r\r\n\r\n\n\r\n\n\n\r\r\n\n\n\r\r\n\n\r\r\n\n\r\r\r\n\r\n\n\r\r\n\r\r\r\r\n\n\r\r\n\n\r\n\r\n\n\n\n\n\r\r\n\n\r\n\r\r\r\n\r\n\n\n\n\n\r\n\n\r\r\r\n\n\r\n\n\r\n\n\r\r\r\r\n\n\r\n\r\r\r\n\n\r\n\n\r\n\r\n\n\n\n\nctf{pearlpearlpear'
b'l}\r\n'
```

I then copied and pasted all of the `\r`'s and `\n`'s into Cyber Chef replacing `\r` with 0 and `\n` with 1

Then just decode the binary to retrieve the flag

ractf{p34r1_1ns1d3_4_cl4m}


