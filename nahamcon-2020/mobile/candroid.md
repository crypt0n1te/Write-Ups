# CAndroid

Firstly I used `7z` to unzip the `.apk` file.

```
7z x candroid.apk
```

Then I checked strings to see if there was a flag somewhere. Surprisingly, there was.

```
strings resources.arsc | grep flag
```

**flag{4ndr0id_1s_3asy}**