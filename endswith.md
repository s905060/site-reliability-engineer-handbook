# Endswith

It returns True if the string ends with the specified suffix, otherwise return False optionally restricting the matching with the given indices start and end.

### Syntax:
```
str.endswith(suffix[, start[, end]])
```

### Parameters:
suffix -- This could be a string or could also be a tuple of suffixes to look for.

* start -- The slice begins from here.
* end -- The slice ends here.

### Example:
```
#!/usr/bin/python

str = "this is string example....wow!!!";

suffix = "wow!!!";
print str.endswith(suffix)
print str.endswith(suffix,20)

suffix = "is";
print str.endswith(suffix, 2, 4)
print str.endswith(suffix, 2, 6)
```

### Result:
```
True
True
True
False
```