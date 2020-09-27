## Briefing:  
```
you either know it or not take this and get your flag

5552415c2b3525105a4657071b3e0b5f494b034515
```

As the name suggests, it's XOR. 

We know the plaintext starts with `darkCTF{` so using this as the key as so you'll get the actual key outputted- see [here](https://gchq.github.io/CyberChef/#recipe=From_Hex('Auto')XOR(%7B'option':'UTF8','string':'darkCTF%7B'%7D,'Standard',false)&input=NTU1MjQxNWMyYjM1MjUxMDVhNDY1NzA3MWIzZTBiNWY0OTRiMDM0NTE1)

Therefore using the key `1337hack` gives us the flag- see [here](https://gchq.github.io/CyberChef/#recipe=From_Hex('Auto')XOR(%7B'option':'UTF8','string':'1337hack'%7D,'Standard',false)&input=NTU1MjQxNWMyYjM1MjUxMDVhNDY1NzA3MWIzZTBiNWY0OTRiMDM0NTE1)

### Flag:  
`darkCTF{kud0s_h4xx0r}`

(By Ironstone)
