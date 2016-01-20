# Method overriding in Python

What is overriding? Overriding is the ability of a class to change the implementation of a method provided by one of its ancestors.

Overriding is a very important part of OOP since it is the feature that makes inheritance exploit its full power. Through method overriding a class may "copy" another class, avoiding duplicated code, and at the same time enhance or customize part of it. Method overriding is thus a strict part of the inheritance mechanism.

###A quick glance to inheritance
As for most OOP languages, in Python inheritance works through implicit delegation: when the object cannot satisfy a request, it first tries to forward the request to its ancestors, following the specific language rules in the case of multiple inheritance.

An exemple:

```
class Parent(object):
    def __init__(self):
        self.value = 5

    def get_value(self):
        return self.value

class Child(Parent):
    pass
```

As you can see the Child class is empty, but since it inherits from Parent Python takes charge of routing all method calls. So you may use the get_value() method of Child objects and exerything works as expected.
```
>>> c = Child()
>>> c.get_value()
5
```
Indeed get_value() is not exactly part of the Child class as if it were defined in it

```
>>> p = Parent()
>>> c = Child()
>>>
>>> dir(p)
['__class__', '__delattr__', '__dict__', '__doc__', '__format__',
 '__getattribute__', '__hash__', '__init__', '__module__', '__new__',
 '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__',
 '__str__', '__subclasshook__', '__weakref__', 'get_value', 'value']
>>>
>>> dir(c)
['__class__', '__delattr__', '__dict__', '__doc__', '__format__',
 '__getattribute__', '__hash__', '__init__', '__module__', '__new__',
 '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__',
 '__str__', '__subclasshook__', '__weakref__', 'get_value', 'value']
>>>
>>> dir(Parent)
['__class__', '__delattr__', '__dict__', '__doc__', '__format__',
 '__getattribute__', '__hash__', '__init__', '__module__', '__new__',
 '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__',
 '__str__', '__subclasshook__', '__weakref__', 'get_value']
>>>
>>> dir(Child)
['__class__', '__delattr__', '__dict__', '__doc__', '__format__',
 '__getattribute__', '__hash__', '__init__', '__module__', '__new__',
 '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__',
 '__str__', '__subclasshook__', '__weakref__', 'get_value']
>>>
>>> Parent.__dict__
dict_proxy({'__module__': '__main__',
            'get_value': <function get_value at 0xb69a656c>,
            '__dict__': <attribute '__dict__' of 'Parent' objects>,
            '__weakref__': <attribute '__weakref__' of 'Parent' objects>,
            '__doc__': None,
            '__init__': <function __init__ at 0xb69a6534>})
>>>
>>> Child.__dict__
dict_proxy({'__module__': '__main__', '__doc__': None})
```

This shows that the Child class does not actually contain the get_value() method and that a mechanism of automatical delegation is active under the hood. For an insight on this mechanism check this post.

###Method overriding in action
In Python method overriding occours simply defining in the child class a method with the same name of a method in the parent class. When you define a method in the object you make this latter able to satisfy that method call, so the implementations of its ancestors do not come in play.
```
class Parent(object):
    def __init__(self):
        self.value = 5

    def get_value(self):
        return self.value

class Child(Parent):
    def get_value(self):
        return self.value + 1
```

Now Child objects behave differently
```
>>> c = Child()
>>> c.get_value()
6
```
and taking a look inside the class we spot a difference

```
>>> Parent.__dict__
dict_proxy({'__module__': '__main__',
            'get_value': <function get_value at 0xb69a656c>,
            '__dict__': <attribute '__dict__' of 'Parent' objects>,
            '__weakref__': <attribute '__weakref__' of 'Parent' objects>,
            '__doc__': None,
            '__init__': <function __init__ at 0xb69a6534>})
>>>
>>> Child.__dict__
dict_proxy({'__module__': '__main__',
            'get_value': <function get_value at 0xb69a65a4>,
            '__doc__': None})
```