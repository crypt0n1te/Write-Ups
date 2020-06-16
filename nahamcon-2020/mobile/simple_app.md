# Simple App

A with CAndroid, I used `7z` to unzip the `.apk` file.

```
7z x candroid.apk
```

Then I grepped all the files to see if there were any references to `flag`. If there were, I checked them out.

```
grep -rnw "flag"
```
This shows match in "classes.dex"

```
cat classes.dex | grep flag
```

**flag{3asY_4ndr0id_r3vers1ng}**