# Traversing a Directory Tree

os.path.walk() traverses all of the directories in a tree and calls a function you provide passing the directory name and the names of the contents of that directory. This example produces a recursive directory listing, ignoring .svn directories.

```
import os
import os.path
import pprint

def visit(arg, dirname, names):
    print dirname, arg
    for name in names:
        subname = os.path.join(dirname, name)
        if os.path.isdir(subname):
            print '  %s/' % name
        else:
            print '  %s' % name
    print

os.mkdir('example')
os.mkdir('example/one')
f = open('example/one/file.txt', 'wt')
f.write('contents')
f.close()
f = open('example/two.txt', 'wt')
f.write('contents')
f.close()
os.path.walk('example', visit, '(User data)')
$ python ospath_walk.py

example (User data)
  one/
  two.txt

example/one (User data)
  file.txt
```