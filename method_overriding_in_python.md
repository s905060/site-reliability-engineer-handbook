# Method overriding in Python

What is overriding? Overriding is the ability of a class to change the implementation of a method provided by one of its ancestors.

Overriding is a very important part of OOP since it is the feature that makes inheritance exploit its full power. Through method overriding a class may "copy" another class, avoiding duplicated code, and at the same time enhance or customize part of it. Method overriding is thus a strict part of the inheritance mechanism.

###A quick glance to inheritance
As for most OOP languages, in Python inheritance works through implicit delegation: when the object cannot satisfy a request, it first tries to forward the request to its ancestors, following the specific language rules in the case of multiple inheritance.

An exemple: