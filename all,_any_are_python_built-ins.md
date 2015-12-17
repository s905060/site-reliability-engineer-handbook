# 'all', 'any' are Python built-ins

You can (and should) read about all and any in the Python docs. Also, if you haven't checked out Truthiness in Python yet, you should go look at it so you understand what evaluates to True and False.

```
'all', 'any' check that all/any items in the list are True
```

all and any, in a nutshell, allow you to do the equivalent of and and or respectively but on an entire list.* So, in the most basic case, here's how they work:

```
>>> print all([True, True, True, False, True, True]) 
False
>>> print any([True, True, True, False, True, True])
True
```