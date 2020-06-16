# Read the Rules
Just Read the Rules.

# CLIsay
Running `strings file | grep flag` returns part of the flag, so we run `strings file | grep "flag" -A 10` to get the next 10 lines after the occurence. Or you could just do `strings` and search around a bit.

# Metameme
Extract the metadata, it's all there.

Running `strings` could also work.

# Mr Robot
Just go to `/robots.txt`.

# UGGC
Login as any random "user", notice that a new cookie is made with an odd value. If you decode that value using rot13 it returns the name of your user.
Simply rot13 the string "admin" and pass it in as the new value for the user cookie.

# Easy Keesy
Running `file` shows that it is a KeePass file, running `keepass2john [file] > [new_file]` then `john --wordlist=rockyou.txt [new_file]` cracks it to give the password `monkeys`.

Then install keepass2 (sudo apt install keepass2) and open the original file using that, decrypting with the password `monkeys`.

# Pang
Just run `strings` on the file.
