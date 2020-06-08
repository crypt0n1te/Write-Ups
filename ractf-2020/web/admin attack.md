# Admin Attack

So based on previous challlenges we know that the RARO site is prone to SQL injection. So a good start is to test with some form of `OR 1=1`. So i began to test with the basic combinations you are likely to find.

Upon testing i found that the combination `'OR 1=1;--` gave me access to an account, just not the admin account. Which means we needed to find a way to get access to the `jimmyTehAdmin` account we found in the previous challenge.

### Limit Cause
In SQL we can use the Limit Clause to select a specific item, it is defined by GeeksforGeeks as the following:

`The LIMIT clause is used to set an upper limit on the number of tuples returned by SQL.`

If you carry on reading you will see that it outlines the following:

`LIMIT x OFFSET y simply means skip the first y entries and then return the next x entries.
OFFSET can only be used with ORDER BY clause. It cannot be used on its own.`

So for our purposes we can use a combination of LIMIT and OFFSET to find the account we want, which is `jimmyTehAdmin` so i messed around with these values a little bit and before long, i found the correct combination.

### Final Steps

So now we know the method we will need to use, we can mess around with that until we get what we need. Before, we found that `'OR 1=1;--` was successful, so thats a good place to start.

We know we will need to add the limit and offset somewhere in that statement.

`'OR 1=1 LIMIT 1 OFFSET 0;--`

When we try the above example, we succcessfully login and are greeted by the flag like so:
```
Welcome, jimmyTehAdmin
ractf{!!!4dm1n4buse!!!}
```

> Created by Christopher Harris (cjharris18) on the 8th June 2020.
