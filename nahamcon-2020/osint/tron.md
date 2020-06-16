# Tron
So first rag went around to common social media sites, and eventually found a [GitHub account](https://github.com/NahamConTron).

There was a pinned repository called [Flagger](https://github.com/NahamConTron/flagger). The directory was analysed, but nothing interesting was found except the description.

> Capture the Flag service to collect flags
> 
> Sorry, your flag is in another castle.

At this point he had to go and I took over.

I decided to check his other repositories, one of which was a [fork of a dotfiles repo](https://github.com/NahamConTron/dotfiles). Since the other repo was a dead end, I decided to check this one out.

Something that caught my eye was the descriptive note GitHub added:

> This branch is 2 commits ahead of calebstewart:master.

If it was two commits ahead, then this account must have mades those two changes. I clicked the `Compare` button next to this to check the changes, and they were very interesting.

One of these was an OpenSSH private key.

```
-----BEGIN OPENSSH PRIVATE KEY-----
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
xTSB/z4XAJNBkAAAAKam9obkB4cHMxNQE=
-----END OPENSSH PRIVATE KEY-----
```

Another was in `.bash_history`:

```s
ssh -i config/id_rsa nahamcontron@jh2i.com -p 50033
```

So this was fairly clear; this key was used to SSH into the website on port 50033. I copied the SSH key into a file called `key`, ran `chmod 700` to change the permissions so that it allowed me to use it to SSH, and connected.

```s
ssh -i key nahamcontron@jh2i.com -p 50033
```

Once in, all we had to do was `cat flag.txt`.

**flag{nahamcontron_is_on_the_grid}**