# Unitest

###unittest example

Using the same unnecessary_math.py module that I wrote in the
doctest intro, here’s some example test
code to test my ‘multiply’ function.
```
test_um_unittest.py:
```
```
import unittest
from unnecessary_math import multiply
 
class TestUM(unittest.TestCase):
 
    def setUp(self):
        pass
 
    def test_numbers_3_4(self):
        self.assertEqual( multiply(3,4), 12)
 
    def test_strings_a_3(self):
        self.assertEqual( multiply('a',3), 'aaa')
 
if __name__ == '__main__':
    unittest.main()
```

In this example, I’ve used assertEqual(). The unittest framework has a whole bunch of assertBlah() style functions like assertEqual(). Once you have a reasonable reference for all of the assert functions bookmarked, working with unnittest is pretty powerful and easy.

Aside from the tests you write, most of what you need to do can be accomplished with the test fixture methods such as setUp, tearDown, setUpClass, tearDownClass, etc.

###Running unittests

At the bottom of the test file, we have this code:


 
1
2
if __name__ == '__main__':
    unittest.main()

This allows us to run all of the test code just by running the file.
Running it with no options is the most terse, and running with a ‘-v’ is more verbose, showing which tests
ran.


1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
 
> python test_um_unittest.py
..
----------------------------------------------------------------------
Ran 2 tests in 0.000s
 
OK
> python test_um_unittest.py -v
test_numbers_3_4 (__main__.TestUM) ... ok
test_strings_a_3 (__main__.TestUM) ... ok
 
----------------------------------------------------------------------
Ran 2 tests in 0.000s
 
OK
 
Test discovery

Let’s say that you’ve got a bunch of test files. It would be annoying to have to run each test file separately. That’s where test discovery comes in handy.

In our case, all of my test code (one file for now) is in ‘simple_example’.
To run all of the unittests in there, use python -m unittest discover simple_example, with or without the ‘-v’, like this:


1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
 
> python -m unittest discover simple_example
..
----------------------------------------------------------------------
Ran 2 tests in 0.000s
 
OK
> python -m unittest discover -v simple_example
test_numbers_3_4 (test_um_unittest.TestUM) ... ok
test_strings_a_3 (test_um_unittest.TestUM) ... ok
 
----------------------------------------------------------------------
Ran 2 tests in 0.000s
 
OK
 
unittest example with markdown.py

Now, I’ll throw unittest at my markdown.py project.
This is going to be pretty straightforward, as the tests are quite similar to the doctest versions, just formatted with all of the unittest boilerplate stuff, especially since I don’t need to make use of startUp or tearDown fixtures.

test_markdown_unittest.py:


1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
36
 
import unittest
from markdown_adapter import run_markdown
 
class TestMarkdownPy(unittest.TestCase):
 
    def setUp(self):
        pass
 
    def test_non_marked_lines(self):
        '''
        Non-marked lines should only get 'p' tags around all input
        '''
        self.assertEqual(
                run_markdown('this line has no special handling'),
                '<p>this line has no special handling</p>')
 
    def test_em(self):
        '''
        Lines surrounded by asterisks should be wrapped in 'em' tags
        '''
        self.assertEqual(
                run_markdown('*this should be wrapped in em tags*'),
                '<p><em>this should be wrapped in em tags</em></p>')
 
    def test_strong(self):
        '''
        Lines surrounded by double asterisks should be wrapped in 'strong' tags
        '''
        self.assertEqual(
                run_markdown('**this should be wrapped in strong tags**'),
                '<p><strong>this should be wrapped in strong tags</strong></p>')
 
if __name__ == '__main__':
    unittest.main()
 
Testing markdown.py

And now we can see that everything is failing (as expected).


1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
 
> python test_markdown_unittest.py
FFF
======================================================================
FAIL: test_em (__main__.TestMarkdownPy)
----------------------------------------------------------------------
Traceback (most recent call last):
  File "test_markdown_unittest.py", line 29, in test_em
    '<p><em>this should be wrapped in em tags</em></p>')
AssertionError: '*this should be wrapped in em tags*' != '<p><em>this should be wrapped in em tags</em></p>'
 
======================================================================
FAIL: test_non_marked_lines (__main__.TestMarkdownPy)
----------------------------------------------------------------------
Traceback (most recent call last):
  File "test_markdown_unittest.py", line 21, in test_non_marked_lines
    '<p>this line has no special handling</p>')
AssertionError: 'this line has no special handling' != '<p>this line has no special handling</p>'
 
======================================================================
FAIL: test_strong (__main__.TestMarkdownPy)
----------------------------------------------------------------------
Traceback (most recent call last):
  File "test_markdown_unittest.py", line 37, in test_strong
    '<p><strong>this should be wrapped in strong tags</strong></p>')
AssertionError: '**this should be wrapped in strong tags**' != '<p><strong>this should be wrapped in strong tags</strong></p>'
 
----------------------------------------------------------------------
Ran 3 tests in 0.142s
 
FAILED (failures=3)
 
One interesting thing to note as compared to doctest. Only actual tests are counted.
I have 3 tests. And unittest gets that right.
Doctest lists 4 tests, with one of them passing. What’s the 4th? It’s the import statement.
Every statement is counted in doctest, so the counts are quite a bit wacky, if you ask me.
The counts are way more meaningful in unittest.