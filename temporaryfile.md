# TemporaryFile

If your application needs a temporary file to store data, but does not need to share that file with other programs, the best option for creating the file is the TemporaryFile() function. It creates a file, and on platforms where it is possible, unlinks it immediately. This makes it impossible for another program to find or open the file, since there is no reference to it in the filesystem table. The file created by TemporaryFile() is removed automatically when it is closed.

```
import os
import tempfile

print 'Building a file name yourself:'
filename = '/tmp/guess_my_name.%s.txt' % os.getpid()
temp = open(filename, 'w+b')
try:
    print 'temp:', temp
    print 'temp.name:', temp.name
finally:
    temp.close()
    # Clean up the temporary file yourself
    os.remove(filename)

print
print 'TemporaryFile:'
temp = tempfile.TemporaryFile()
try:
    print 'temp:', temp
    print 'temp.name:', temp.name
finally:
    # Automatically cleans up the file
    temp.close()
```

This example illustrates the difference in creating a temporary file using a common pattern for making up a name, versus using the TemporaryFile() function. Notice that the file returned by TemporaryFile() has no name.

```
$ python tempfile_TemporaryFile.py
```

Building a file name yourself:
```
temp: <open file '/tmp/guess_my_name.14891.txt', mode 'w+b' at 0x100458270>
temp.name: /tmp/guess_my_name.14891.txt
```

TemporaryFile:
```
temp: <open file '<fdopen>', mode 'w+b' at 0x100458780>
temp.name: <fdopen>
```

By default, the file handle is created with mode 'w+b' so it behaves consistently on all platforms and your program can write to it and read from it.
```
import os
import tempfile

temp = tempfile.TemporaryFile()
try:
    temp.write('Some data')
    temp.seek(0)
    
    print temp.read()
finally:
    temp.close()
```

After writing, you have to rewind the file handle using seek() in order to read the data back from it.
```
$ python tempfile_TemporaryFile_binary.py
```

Some data
If you want the file to work in text mode, set mode to 'w+t' when you create it:
```
import tempfile

f = tempfile.TemporaryFile(mode='w+t')
try:
    f.writelines(['first\n', 'second\n'])
    f.seek(0)

    for line in f:
        print line.rstrip()
finally:
    f.close()
```

The file handle treats the data as text:
```
$ python tempfile_TemporaryFile_text.py

first
second
```