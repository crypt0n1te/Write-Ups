# Insert witty name

> Having access to the site's source would be really useful, but we don't know how we could get it. All we know is that the site runs python.

So this challenge was pretty simple, from previous challenges we saw that visiting `/static?f=backup.txt` would return the `backup.txt` file that was mentioned in Entrypoint.

If you can get the site to return you one unintended file why not another? Simply visiting `/static?f=app.py` (There was no way of knowing the name of this file as far as I can tell, but we know from the challenge brief that the server runs python, and app.py is a very common name for python servers) and voila, we get a prompt to download a python file containing the flag.