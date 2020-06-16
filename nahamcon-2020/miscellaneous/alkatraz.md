# Alkatraz
We can't use `cat` to read the file, so we'll have to find another way.

```sh
while read line; do echo $line; done < flag.txt
```