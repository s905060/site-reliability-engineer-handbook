# OS

os.times()
Return a 5-tuple of floating point numbers indicating accumulated (processor or other) times, in seconds. The items are: user time, system time, children’s user time, children’s system time, and elapsed real time since a fixed point in the past, in that order. See the Unix manual page times(2) or the corresponding Windows Platform API documentation. On Windows, only the first two items are filled, the others are zero.

Availability: Unix, Windows

```
#!/usr/bin/python

import os

for param in os.environ.keys():
    print "%20s %s" % (param,os.environ[param])
```