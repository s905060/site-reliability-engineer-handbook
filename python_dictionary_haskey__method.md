# Python dictionary has_key() Method

The method has_key() returns true if a given key is available in the dictionary, otherwise it returns a false.

####Syntax
Following is the syntax for has_key() method −
```
dict.has_key(key)
```

####Parameters
key -- This is the Key to be searched in the dictionary.

####Return Value
This method return true if a given key is available in the dictionary, otherwise it returns a false.

####Example
The following example shows the usage of has_key() method.
```
#!/usr/bin/python

dict = {'Name': 'Zara', 'Age': 7}

print "Value : %s" %  dict.has_key('Age')
print "Value : %s" %  dict.has_key('Sex')
```

When we run above program, it produces following result −
```
Value : True
Value : False
```