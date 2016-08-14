# DICTIONARY COMPREHENSION

[xv if c else yv for (c,xv,yv) in zip(condition,x,y)]



## zip

Suppose we have two lists, we want to make them a list making pairs with . How can we do that?
```python
>>> a = [1,2,3]
>>> b = [4,5,6]
```

For Python 2.x:
```python
>>> z = zip(a,b)
>>> z
[(1, 4), (2, 5), (3, 6)]
```

For Python 3.x:
```python
>>> z = list(zip(a,b))
>>> z
[(1, 4), (2, 5), (3, 6)]
```

Then, we reverse the course, and want to get each list back. How?
```python
>>> c, d = zip(*z)
>>> c, d
((1, 2, 3), (4, 5, 6))
```


## Dictionary construction with zip

We can use zip to generate dictionaries when the keys and values must be computed at runtime.

In general, we can make a dictionary by typing in the dictionary literals:
```python
>>> D1 = {'a':1, 'b':2, 'c':3}
>>> D1
{'a': 1, 'c': 3, 'b': 2}
>>> 
```

Or we can make it by assigning values to keys:
```python
>>> D1 = {}
>>> D1['a'] = 1
>>> D1['b'] = 2
>>> D1['c'] = 3
>>> D1
{'a': 1, 'c': 3, 'b': 2}
```

However, there are cases when we get the keys and values in lists at runtime. For instance like this:
```python
>>> keys = ['a', 'b', 'c']
>>> values = [1, 2, 3]
```


## How can we construct a dictionary from those lists of keys and values?


Now it's time to use zip. First, we zip the lists and loop through them in parallel like this:
```python
>>> list(zip(keys,values))
[('a', 1), ('b', 2), ('c', 3)]

>>> D2 = {}
>>> for (k,v) in zip(keys, values):
...     D2[k] = v
... 
>>> D2
{'a': 1, 'b': 2, 'c': 3}
```

Note that making a dictionary like that only works for Python 3.x.

There is another way of constructing a dictionary via zip that's working for both Python 2.x and 3.x. We make a dict from zip result:
```python
>>> D3 = dict(zip(keys, values))
>>> D3
{'a': 1, 'b': 2, 'c': 3}
```

Python 3.x introduced dictionary comprehension, and we'll see how it handles the similar case. Let's move to the next section.


## Dictionary Comprehension


As in the previous section, we have two lists:
```python
>>> keys = ['a', 'b', 'c']
>>> values = [1, 2, 3]
```

Now we use dictionary comprehension (Python 3.x) to make dictionary from those two lists:
```python
>>> D = { k:v for (k,v) in zip(keys, values)}
>>> D
{'a': 1, 'b': 2, 'c': 3}
```

It seems require more code than just doing this:
```python
>>> D = dict(zip(keys, values))
>>> D
{'a': 1, 'b': 2, 'c': 3}
```

However, there are more cases when we can utilize the dictionary comprehension. For instance, we can construct dictionary from one list using comprehension:
```python
>>> D = {x: x**2 for x in [1,2,3,4,5]}
>>> D
{1: 1, 2: 4, 3: 9, 4: 16, 5: 25}

>>> D = {x.upper(): x*3 for x in 'abcd'}
>>> D
{'A': 'aaa', 'C': 'ccc', 'B': 'bbb', 'D': 'ddd'}
```

When we want initialize a dict from keys, we do this:
```python
>>> D = dict.fromkeys(['a','b','c'], 0)
>>> D
{'a': 0, 'c': 0, 'b': 0}
```

We can use dictionary comprehension to do the same thing;

```python
>>> D = {k: 0 for k in ['a','b','c']}
>>> D
{'a': 0, 'c': 0, 'b': 0}
```

We sometimes generate a dict by iterating each element:
```python
>>> D = dict.fromkeys('dictionary')
>>> D
{'a': None, 'c': None, 'd': None, 'i': None, 'o': None, 'n': None, 'r': None, 't': None, 'y': None}
```

If we use comprehension:
```python
>>> D = {k:None for k in 'dictionary'}
>>> D
{'a': None, 'c': None, 'd': None, 'i': None, 'o': None, 'n': None, 'r': None, 't': None, 'y': None}
```


## Simple zip()

The following example calculates Hamming distance. Hamming distance is the number of positions at which the corresponding symbols are different. It's defined for two strings of equal length.
```python
def hamming(s1,s2):
    if len(s1) != len(s2):
        raise ValueError("Not defined - unequal lenght sequences")
    return sum(c1 != c2 for c1,c2 in zip(s1,s2))

if __name__ == '__main__':
    print(hamming('toned', 'roses'))            # 3
    print(hamming('2173896', '2233796'))        # 3
    print(hamming('0100101000', '1101010100'))  # 6
```


## Conditional zip()
```python
>>> x = [1,2,3,4,5]
>>> y = [11,12,13,14,15]
>>> condition = [True,False,True,False,True]
>>> [xv if c else yv for (c,xv,yv) in zip(condition,x,y)]
[1, 12, 3, 14, 5]
```

The same thing can be done using NumPy's where:
```python
>>> import numpy as np
>>> np.where([1,0,1,0,1], np.arange(1,6), np.arange(11,16))
array([ 1, 12,  3, 14,  5])
```