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

The first example isn't true because in not all of the list is True, the second example is True because at least one of the list is True. You could rewrite this to an if comparison, e.g.:

```
>>> print "True" if (True and True and True and False and True and True) else "False" 
False 
>>> if (True or True or True or False or True or True): print "True"
True
```

so this can be a really useful way to check a whole list of values and then use them. Further, you can check them all in one line and then work with the data, etc.

###NOTE: 'all' and 'any' short-circuit

NOTE: all and any short-circuit, so if it's important that *every* operation in a list of operations is completed, calculate the list first, THEN check it with all and any. e.g.

```
>>> q = Queue(50)
>>> print any([q.enqueue(1), q.enqueue(2) ... q.enqueue(50)])  
True
>>> print q.size
1
```

The operation short-circuits (stops) after the first True value (or, for any, the first False value). If you just calculate the list first, then use any or all on it, you'll be all good. E.g.

```
>>> q = Queue(5)
>>> results = [q.enqueue(1), q.enqueue(2), q.enqueue(3)]
>>> print any(results) 
True
>>> print q.size 
3
```

###Aside: all and any take iterables (and sequences)

Specifically, they take an iterable that produces elements and they both short-circuit (check out the all to see an equivalent-code example in Python, but for right now easier and simpler to talk about lists specifically.

What's the difference? It means that you don't have to instantiate a list first, you can just give it a generator expression (or any iterable), e.g.

```
def counting_generator():
    counting_generator.count += 1
    if counting_generator.count < 100:
        yield True

# given counting_generator its counter
counting_generator.count = 0

>>> print "The result was %r and produced %d items" % (any(counting_generator()), counting_generator.count)
The result was True and produced 1 items.
>>> counting_generator.count = 0
>>> # if you don't include the if statement this will be an infinite loop!
>>> print "The result was %r and produced %d items" % (all(counting_generator()), counting_generator.count)
The result was True and produced 100 items.
```