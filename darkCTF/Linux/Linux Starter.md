## Briefing: 

`Don't Try to break this jail.     
ssh wolfie@linuxstarter.darkarmy.xyz -p 8001 password : wolfie` 

---

Sshing in and running `echo $SHELL` shows us we have an rbash shell- that is, a restricted shell. 

Googling how to bypass this I found you could add 'bash --noprofile' to the end of the ssh command. 

So running `ssh wolfie@linuxstarter.darkarmy.xyz -p 8001 'bash --noprofile'` gives us an unrestricted shell. 
From there, just `cd imp` and `cat flag.txt` to get the flag.

### Flag:   
```darkCTF{h0pe_y0u_used_intended_w4y}```

