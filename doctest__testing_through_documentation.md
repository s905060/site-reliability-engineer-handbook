# doctest – Testing through documentation

The first step to setting up doctests is to use the interactive interpreter to create examples and then copy and paste them into the docstrings in your module. Here, my_function() has two examples given:

```python
def my_function(a, b):
    """
    >>> my_function(2, 3)
    6
    >>> my_function('a', 3)
    'aaa'
    """
    return a * b
```

To run the tests, use doctest as the main program via the -m option to the interpreter. Usually no output is produced while the tests are running, so the example below includes the -v option to make the output more verbose.

```python
$ python -m doctest -v doctest_simple.py

Trying:
    my_function(2, 3)
Expecting:
    6
ok
Trying:
    my_function('a', 3)
Expecting:
    'aaa'
ok
1 items had no tests:
    doctest_simple
1 items passed all tests:
   2 tests in doctest_simple.my_function
2 tests in 2 items.
2 passed and 0 failed.
Test passed.
```