# Snakes and Ladders

For this I wrote a decrypt function to reverse wha the encrypted function did.

```python
encrypted_flag = "fqtbjfub4uj_0_d00151a52523e510f3e50521814141c"

def sxor(s1,s2):
    return ''.join(chr(ord(a) ^ ord(b)) for a,b in zip(s1,s2))

def decrypt(a):
    flag = ''
    for char in a[:15]:
        if char >= 'a' and char <= 'z':
            char = chr(ord(char)-14)
            if char < 'a':
                char = chr(ord(char)+26)
        flag += char
    hexpart = sxor('aaaaaaaaaaaaaaa', bytearray.fromhex(a[15:]).decode())
    for i in range(0,15):
            print(flag[i], end='')
            print(hexpart[i],end='')
            
decrypt(encrypted_flag)
```

**ractf{n3v3r_g0nn4_g1v3_y0u_up}**