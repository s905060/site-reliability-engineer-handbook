# Map, Reduce, Zip, Filter

The **map**, **reduce**, **filter**, and **zip** built-in functions are handy functions for processing sequences. These are related to functional programming languages. The idea is to take a small function you write and apply it to all the elements of a sequence. This saves you writing an explicit loop. The implicit loop within each of these functions may be faster than an explicit for or while loop.


## map

map ( function , sequence , [ sequence... ] ) → list
Create a new list from the results of applying the given function to the items of the given sequence . If more than one sequence is given, the function is called with multiple arguments, consisting of the corresponding item of each sequence. If any sequence is too short, None is used for missing value. If the function is None, map will create tuples from corresponding items in each list, much like the zip function.

```python
Example:
>>> map(lambda a: a+1, [1,2,3,4])
[2, 3, 4, 5]
>>> map(lambda a, b: a+b, [1,2,3,4], (2,3,4,5))
[3, 5, 7, 9]
>>> map(lambda a, b: a + b if b else a + 10, [1,2,3,4,5], (2,3,4,5))   ＃ the second iterable list is one item short
[3, 5, 7, 9, 15]
>>> map(None, [1,2,3,4,5], [1,2,3])

[(1, 1), (2, 2), (3, 3), (4, None), (5, None)]
```


## reduce
 

reduce(function, sequence[, initial]) -> value

Apply a function of two arguments cumulatively to the items of a sequence, from left to right, so as to reduce the sequence to a single value.
For example, reduce(lambda x, y: x+y, [1, 2, 3, 4, 5]) calculates ((((1+2)+3)+4)+5).  If initial is present, it is placed before the items of the sequence in the calculation, and serves as a default when the sequence is empty.
Example:
```python
>>> reduce(lambda x, y: x+y, range(0,10))
45
>>> reduce(lambda x, y: x+y, range(0,10), 10)
55
```


## filter

Namespace:  Python builtin
Docstring:
filter(function or None, sequence) -> list, tuple, or string

Return those items of sequence for which function(item) is true.  If
function is None, return the items that are true.  If sequence is a tuple

or string, return the same type, else return a list.
Example: 
```python
>>> filter(lambda d: d != 'a', 'abcd')　　＃ filter out letter 'a'。
'bcd'
>>> def d(x):　＃ not using lambda function, instead using a predefined function 
 　　　　　return True if x != 'a' else False
>>> filter(d, 'abcd')
'bcd'
```


## zip

zip(seq1 [, seq2 [...]]) -> [(seq1[0], seq2[0] ...), (...)]

Return a list of tuples, where each tuple contains the i-th element
from each of the argument sequences.  The returned list is truncated

in length to the length of the shortest argument sequence.

Example: 
```python
>>> 
zip( range(5), range(1,20,2) )

[(0, 1), (1, 3), (2, 5), (3, 7), (4, 9)]
```