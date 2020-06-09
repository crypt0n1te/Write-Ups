# Disk Forensics fun

## First Steps:
We are told this a disk forensics challenge, which means the first steps are to open an Autopsy case, I won't cover the specifics of this but if you are unfamiliar with Autopsy i reccomend you check out the following:
https://sleuthkit.org/autopsy/docs/user-docs/4.0/quick_start_guide.html

### Looking for the files:
So the first steps are to start looking for interesting files. I always start by looking at if the Ingest Modules return anything, in this case, nothing of use. So I begin manually analysing the files, this is what forensics is all about :mag:

The other thing I notice is that a lot of the files are empty, which seems strange to me so I begin to dig deeper :pick:.

I find that in the Home Directory there are the following files:

```
TRACK2.M4A
TRACK.M4A
NOTHINGH.asc
```
The .asc file seems interesting so i extract it to my Desktop to further analyse.

Looking at the file i see that it is a PGP message, as it says at the top of the file here:

````
-----BEGIN PGP MESSAGE-----
```

A bit of research leads me to find the following link:
https://fileinfo.com/extension/pgp

This explains to me that a PGP file is the following:

**Security key or digital signature file that verifies a user's identity; used for decrypting a file encrypted with Pretty Good Privacy (PGP) software; ensures that protected files can only be opened by authorized users.**

So i begin to look for these keys, a quick keyword search for `pgp` gives me these 2 files, in the `ROOT` Directory:
```
PRIVATE.PGP
PUBLIC.PGP
```

So i extract these files  to my desktop also, it seems we have a lead.

### Decrypting the file using the keys:
So upon further research i find that Linux has a gpg command, the man page for which is here:
https://gnupg.org/documentation/manpage.html

So i start by running the command `gpg --import [.pgp file]` on both the `[.pgp files]` then i run the following command on the `[.asc file]`: `gpg -e [.asc file] -o output.html`

### Finishing Steps:
So now i open the HTML file in my browser and i get what looks like a file. So I then just put this into an online hex to file creator, like the one found here:
https://tomeko.net/online_tools/hex_to_file.php?lang=en

Then its as simple as opening the file, and you have the flag.

> Created on the 9th June by Christopher Harris (@cjharris18).
