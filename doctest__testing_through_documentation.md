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

You can class test with doctest. One issue to consider is to create object to test the method. This is an example. 

```python
"""
This is the "iniFileGenerator" module.
 
>>> f = iniFileGenerator()
>>> f.get()
hello
"""

class iniFileGenerator:
    def __ini__(self, hintFilePath):
        self.hintFilePath = hintFilePath
    def get(self):
        """
        >>> iniFileGenerator().get()
        hello
        """
        print "hello"
         
if __name__ == "__main__":
    import doctest
    doctest.testmod()
```

No news is good news, if you want to get the test result you need to use command line. 
```python
refactoringChecker> python iniFileGenerator.py -v
```
```python
Trying:
    f = iniFileGenerator()
Expecting nothing
ok
Trying:
    f.get()
Expecting:
    hello
ok
Trying:
    iniFileGenerator().get()
Expecting:
    hello
ok
2 items had no tests:
    __main__.iniFileGenerator
    __main__.iniFileGenerator.__ini__
2 items passed all tests:
   2 tests in __main__
   1 tests in __main__.iniFileGenerator.get
3 tests in 4 items.
3 passed and 0 failed.
Test passed.
```

####Reuse testing class objects
When testing class, you want to use the same object multiple times throughout the method testing. In this case, you can use extra globs dictionary. 

```python
"""
This is the "iniFileGenerator" module.
>>> print f.hintFilePath
./tests/unit_test_files/hint.txt
"""
class iniFileGenerator:
    def __init__(self, hintFilePath):
        self.hintFilePath = hintFilePath
    def hello(self):
        """
        >>> f.hello()
        hello
        """
        print "hello"
if __name__ == "__main__":
    import doctest
    hintFile = "./tests/unit_test_files/hint.txt"
    doctest.testmod(extraglobs={'f': iniFileGenerator(hintFile)})
```