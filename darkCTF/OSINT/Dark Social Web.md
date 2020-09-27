## Briefing:  

```
0xDarkArmy has 1 social account and DarkArmy uses the same name everywhere. Hint: The front page of internet
```

First off, running `python3 sherlock 0xDarkArmy` gives us hits on reddit, instagram and twitter, among others. 

While nothing interesting was found on the twitter or instagram, there was a qr code posted on the reddit page, seen [here](https://www.reddit.com/user/0xDarkArmy/)

Scanning the qr code, we are directed to a `.onion` site, openable in tor. see [here](http://cwpi3mxjk7toz7i4.onion/)

At a first look it seems like a static template page. However navigating to `/robots.txt` we get half of the flag: `darkctf{S0c1a1_D04k_`

Opening up developer tools and going to the 'networks' tab, we can see that in the get request to the page, there is a custom HTTP header `Flag: ` under Date. This contains the second half of the flag: `_w3b_051n7}`

### Flag:  
```darkctf{S0c1a1_D04k_w3b_051n7}```


