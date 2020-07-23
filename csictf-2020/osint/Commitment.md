## Commitment ##



We're given the briefing: 
```hoshimaseok is up to no good. Track him down.```

A quick google of the user `hoshimaseok` leads us to a GitHub page, found [here](https://github.com/hoshimaseok).

We see two repositories, one called `SomethingFishy`. Fishy it looks indeed. 

A quick glance inside this repo reveals nothing. However considering the title, Commitment, maybe there is a commit somewhere with the flag. 

We can see that there are two branches- `master` and `dev`. Upon clicking `compare`, we see that there have indeed been many changes to this repo. 

Scrolling through these, you will find the flag in `index.js`, which was deleted. 

The contents of index.js are as follows: 
``` 
API_KEY = randomapi
FLAG = csictf{sc4r3d_0f_c0mm1tm3nt}
