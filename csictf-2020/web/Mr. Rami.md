## Mr. Rami ##

Briefing:
```
"People who get violent get that way because they can’t communicate."
```

Googling this we see it is a quote from Mr. Robot. Perhaps this is as simple as a `robots.txt` challenge?

Indeed it is. navigating to `/robots.txt`, we see.  
```
# Hey there, you're not a robot, yet I see you sniffing through this file.
# SEO you later!
# Now get off my lawn.

Disallow: /fade/to/black
```  

Navigating to `/fade/to/black`, we get the flag:  
`csictf{br0b0t_1s_pr3tty_c00l_1_th1nk}`
