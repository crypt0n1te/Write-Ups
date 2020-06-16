# Homecooked

So first we see that running the program slowly decrypts the string to yield a flag - but it's simply not fast enough. It never ends - we can check by putting a `print()` statement at the end. So, we'll have to somehow make it go faster - and to do this we need to work out what the functions do.

The `b()` function looks like this:

```python
def b(num):
    my_str = str(num)
    rev_str = reversed(my_str)
    if list(my_str) == list(rev_str):
        return True
    else:
        return False
```

If we have a look at what it's doing, it seems to be reversing the input and comparing the two - so it returns `True` if the inputted number is palidromic, and `False` if it is not. We can't really make this more efficient, at least noticably.

The `a()` function, however, looks like this:

```python
def a(num):
    if (num > 1):
        for i in range(2, ceil(sqrt(num))):
            if (num % i) == 0:
                return False
                break
        return True
    else:
        return False
```

What this seems to be doing is looping through every value from `2` to `n-1` and checking if `n` is divisible by it - a way of **checking if it's prime**. However, this is incredibly inefficient, and is probably the reason it takes so long. Let's make it more efficient and see if it does something.

We are going to use the `sympy` function `isprime` to change `a()`:
```python
from sympy import isprime

def a(num):
    return isprime(num)
```

Running this program spits out the full flag much faster now.

**flag{pR1m3s_4re_co0ler_Wh3n_pal1nDr0miC}**
