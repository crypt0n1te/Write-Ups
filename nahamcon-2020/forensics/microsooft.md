# Microsooft
As it's a `docx` file we can extract all the individual parts using `binwalk`.

```s
binwalk -e microsooft.docx
```

Then we navigate to `/root/Desktop/_microsooft.docx.extracted/src/`, open up `oof.txt` and `Ctrl + F` for `flag`.

**flag{oof_is_right_why_gfxdata_though}**