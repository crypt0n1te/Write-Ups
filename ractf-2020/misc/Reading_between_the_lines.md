# Reading Between the Lines

### main.c

The code for main.c is given and the indentation's are `an absolute mess` as said in the brief

Although I spent a fair amount of time trying to compile the code and run I eventually went for a new approach

The flag was in fact hidden in the indentations and spaces as binary

### I then wrote a python script to extract all of the codes and create the flag

```
File = open("main.c","r").readlines() # Open the file

strippedFile = []

for line in File:
    newline = line.strip()
    if newline != '':
        strippedFile.append(line)

strippedFile = "".join(strippedFile)

# This removes all the lines which are only newlines

binary=""

for char in strippedFile:
    if char == "\t": # Tabs are 1's
        binary += "1"
    if char == " ": # Spaces are 0's
        binary += "0"
    if char == "\n": # New lines are spaces
        binary += " "

flag = binary.split() # Separate the binary into an array

for binary in flag:
    print(chr(int(binary,2)), end='') # Convert each binary number to decimal and then to its ascii form

print()
```

```
$ python3 extract.py 
ract{R34d1ngBetw33nChar4ct3r5}
```
