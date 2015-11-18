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