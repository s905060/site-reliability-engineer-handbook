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