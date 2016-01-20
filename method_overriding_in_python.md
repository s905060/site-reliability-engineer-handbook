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