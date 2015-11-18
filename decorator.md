# Decorator

In Python, a Decorator is a type of macro that allows you to inject or modify code in functions or classes. I was turned onto this by my friend Matt Chapman at ILM, but never fully grasped the importance.
```
class myDecorator(object):
	def __init__(self, f):
		self.f = f
	def __call__(self):
		print "Entering", self.f.__name__
		self.f()
		print "Exited", self.f.__name__
 
@myDecorator
def aFunction():
	print "aFunction running"
 
aFunction()
```

When you run the code above you will see the following:
```
>>Entering aFunction
>>aFunction running
>>Exited aFunction
```

So when we call a decorated function, we get a completely different behavior. You can wrap any existing functions, here is an example of wrapping functions for error reporting:
```
class catchAll:
	def __init__(self, function):
		self.function = function
 
	def __call__(self, *args):
		try:
			return self.function(*args)
		except Exception, e:
			print "Error: %s" % (e)
 
@catchAll
def unsafe(x):
  return 1 / x
 
print "unsafe(1): ", unsafe(1)
print "unsafe(0): ", unsafe(0)
```

So when we run this and divide by zero we get:
```
unsafe(1):  1
unsafe(0):  Error: integer division or modulo by zero
```

Using decorators you can make sweeping changes to existing code with minimal effort, like the error reporting function above, you could go back and just sprinkle these in older code.

**Example #1**: Calculate how much time a function takes to execute.

```
import time

def timing_function(some_function):
    """
    Outputs the time a function takes to execute.
    """
    def wrapper():
        t1 = time.time()
        some_function()
        t2 = time.time()
        return "Time it took to run the function: " + str((t2-t1)) + "\n"
    return wrapper

@timing_function
def my_function():
    num_list = []
    for x in (range(0,10000)):
        num_list.append(x)
    print "\nSum of all the numbers: " +str((sum(num_list)))

print my_function()
```

This returns the time before you run my_function() as well as the time after. Then we simply subtract the two to see how long it took to run the function.

**Example #2**: Rate limiting the call to a function.
```
from time import sleep

def sleep_decorator(function):
"""
Limits how fast the function is called.
"""
    def wrapper(*args, **kwargs):
        sleep(2)
        return function(*args, **kwargs)
    return wrapper

@sleep_decorator
def print_number(num):
    return num

print print_number(222)

for x in range(1,6):
    print print_number(x)
```

This decorator is used for rate limiting.

These examples have beem taken from the great article Primer on Python Decorators, check it out!

**Example 3**: If you need to debug by printing out the arguments a function was called with, you could write this:

```
def argument_printer(func):
    def _wrapper(*args, **kwargs):
        print "Called with positional arguments: %s" % list(args)
        print "And with keyword arguments: %s" % kwargs
        return func(*args, **kwargs)
    return _wrapper

@argument_printer
def add(x, y):
    return x + y
```