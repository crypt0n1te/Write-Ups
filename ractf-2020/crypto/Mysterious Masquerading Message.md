# Mysterious Masquerading Message

So at the start open the file, and you see a chunk of base64 and some binary, but the file name is id_rsa which is quite misleading because the challenge is nothing to do with rsa, and that the private key isn't actually a private key.

So our first step is to decode the base64 (we stan cyberchef), so plonk it into a decoding tool.

You should see a big chunk of text, which says:

```
If you are reading this, then you probably figured out that it wasn't actually an SSH key but a disguise. So you have made it this far and for that I say well done. It wasn't very hard, that I know, but nevertheless you have still made it here so congrats. Now you are probably reading this and thinking about annoying the person who made this, and you want to read the whole thing to check for clues, but you cant find any. You are starting to get frustrated at the person who made this as they still haven't mentioned anything to do with the challenge, except "well done you have got this far". You start slamming desks, and soon the monitor will follow. You are wondering where this is going and realising it's coming to the end of the paragraph, and you might not have seen anything. I have given you some things, although you will need something else as well good luck.
696e656564746f6f70656e6c6f636b73
696e697469616c69736174696f6e3132
```
Then, have a look at what the binary decodes to:

```90988c9befe5ea3f5a91effe03060a8714dfc20088415570b394ce9cd32be718```

So we now have 2 strings and one big string, of which the two strings are the same length

lets have a look at what the first two hex strings decode too
the first one:
```ineedtoopenlocks```
the second one:
```initialisation12```
Lets have a think. What does initialisation12 remind you of? IV possibly?
and if thats the iv, wheres the key
Oh wait, 'ineedtoopenlocks' sounds an awful lot like a key.
So a key, and iv together sounds like AES
Lets just check the length of the key and iv to see if theyre actually for AES. Yep they're both 32 chars.
So now all you need to do is plonk the 64 char hex string, the key, and the iv into cyberched or a decrypter tool
then you get: flag = ractf{3Asy_F1aG_0n_aEs_rAcTf}

