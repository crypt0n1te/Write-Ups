# Quarantine - Hidden information

Visiting the site `http://95.216.233.106:15009` shows a web page with a Login and Sign-Up option.

Visit `http://95.216.233.106:15009/robots.txt`.

```
User-Agent: *
Disallow: /admin-stash
```

Go to the `Disallow` link and the flag is there.

`http://95.216.233.106:15009/admin-stash`

**ractf{1m_n0t_4_r0b0T}**