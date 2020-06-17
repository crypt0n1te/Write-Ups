# Volatile
The challenge hinted at the need to use the tool `Volatility`.

First we run `volatility -f memdump.raw imageinfo` on the dump to get the OS version. We then use the `cmdscan` command to check the most recently run commands.
```s
volatility -f memdump.raw --profile=Win7SP1x86_23418 cmdscan
```

One of these is
```
echo JCTF{nice_volatility_tricks_bro}
```

**JCTF{nice_volatility_tricks_bro}**