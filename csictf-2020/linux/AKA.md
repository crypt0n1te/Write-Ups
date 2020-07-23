# AKA

Netcat into the given domain an port. Trying a bunch of different commands it appears that none of them do what you'd expect.

Running `alias` shows that theres different aliases for commands which let you open files. Instead of trying to figure them all out I converted the `flag.txt` file to base64 with `base64 flag.txt`. Then it was a simple matter of copying the output and decoding it (I used cyberchef).
