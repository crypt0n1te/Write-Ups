# Microsooft
As it's a `docx` file we can extract all the individual parts using `binwalk`.

```s
binwalk -e microsooft.docx
```

Then we navigate to `/root/Desktop/_microsooft.docx.extracted/src/`, open up `oof.txt` and `Ctrl + F` for `flag`.

**flag{oof_is_right_why_gfxdata_though}**

----
Alternatively, if you don't want to use the command line, change the extension to `.zip` and unzip

Navigate into the correct folder and open `oof.txt`. `ctrl + F` to search for the flag.  
