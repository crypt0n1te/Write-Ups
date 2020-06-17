# Rotten
Rotten simply required you to use a caesar cipher to decode the text it sent you. Luckily, once decoded, all the text contained the word `send` and this allowed us to filter out the correct shift.

```python
import socket
from caesarcipher import CaesarCipher

host = "jh2i.com"
port = 50034


count = 0
flag = [" "] * 30
print(len(flag))

while True:
    s =  socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    s.connect((host, port))
    while True:
        try:
            data = s.recv(1024).decode("utf-8")
            for i in range(1, 27, 1):
                x = CaesarCipher(data, offset=i).encoded
                if "send" in x:
                    break
            
            if count > 0:
                pos = x.split(" ")[6]
                char = x.split(" ")[11].replace("\n","")
                print(pos, char)
                flag[int(pos)] = char

            s.sendall(data.encode())

            if len(data) == 0:
                break
            
            count += 1
        except:    
            print("error", flag)
            s.close()
            count = 0
            break
    if " " not in flag:
        break
        u = ""
        for i in flag:
            u += i
        print(u)
```