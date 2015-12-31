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

###Working with files

The built-in open function lets you create, open, and modify files. This module adds those extra functions you need to rename and remove files:

###Example: Using the os module to rename and remove files

```
import os
import string

def replace(file, search_for, replace_with):
    # replace strings in a text file

    back = os.path.splitext(file)[0] + ".bak"
    temp = os.path.splitext(file)[0] + ".tmp"

    try:
        # remove old temp file, if any
        os.remove(temp)
    except os.error:
        pass

    fi = open(file)
    fo = open(temp, "w")

    for s in fi.readlines():
        fo.write(string.replace(s, search_for, replace_with))

    fi.close()
    fo.close()

    try:
        # remove old backup file, if any
        os.remove(back)
    except os.error:
        pass

    # rename original to backup...
    os.rename(file, back)

    # ...and temporary to original
    os.rename(temp, file)

#
# try it out!

file = "samples/sample.txt"

replace(file, "hello", "tjena")
replace(file, "tjena", "hello")
```

###Working with directories

The os module also contains a number of functions that work on entire directories.

The listdir function returns a list of all filenames in a given directory. The current and parent directory markers used on Unix and Windows (. and ..) are not included in this list.

###Example: Using the os module to list the files in a directory

```
import os

for file in os.listdir("samples"):
    print file

sample.au
sample.jpg
sample.wav
...
```
The getcwd and chdir functions are used to get and set the current directory:

###Example: Using the os module to change the working directory
```
import os

# where are we?
cwd = os.getcwd()
print "1", cwd

# go down
os.chdir("samples")
print "2", os.getcwd()

# go back up
os.chdir(os.pardir)
print "3", os.getcwd()

1 /ematter/librarybook
2 /ematter/librarybook/samples
3 /ematter/librarybook
```
The makedirs and removedirs functions are used to create and remove directory hierarchies.

###Example: Using the os module to create and remove multiple directory levels
```
import os

os.makedirs("test/multiple/levels")

fp = open("test/multiple/levels/file", "w")
fp.write("inspector praline")
fp.close()

# remove the file
os.remove("test/multiple/levels/file")

# and all empty directories above it
os.removedirs("test/multiple/levels")
```

Note that removedirs removes all empty directories along the given path, starting with the last directory in the given path name. In contrast, the mkdir and rmdir functions can only handle a single directory level.

###Example: Using the os module to create and remove directories
```
import os

os.mkdir("test")
os.rmdir("test")

os.rmdir("samples") # this will fail

Traceback (innermost last):
  File "os-example-7", line 6, in ?
OSError: [Errno 41] Directory not empty: 'samples'
```
To remove non-empty directories, you can use the rmtree function in the shutil module.

###Working with file attributes

The stat function fetches information about an existing file. It returns a 9-tuple which contains the size, inode change timestamp, modification timestamp, and access privileges.

###Using the os module to get information about a file
```
import os
import time

file = "samples/sample.jpg"

def dump(st):
    mode, ino, dev, nlink, uid, gid, size, atime, mtime, ctime = st
    print "- size:", size, "bytes"
    print "- owner:", uid, gid
    print "- created:", time.ctime(ctime)
    print "- last accessed:", time.ctime(atime)
    print "- last modified:", time.ctime(mtime)
    print "- mode:", oct(mode)
    print "- inode/dev:", ino, dev

#
# get stats for a filename

st = os.stat(file)

print "stat", file
dump(st)
print

#
# get stats for an open file

fp = open(file)

st = os.fstat(fp.fileno())

print "fstat", file
dump(st)
```

```
stat samples/sample.jpg
- size: 4762 bytes
- owner: 0 0
- created: Tue Sep 07 22:45:58 1999
- last accessed: Sun Sep 19 00:00:00 1999
- last modified: Sun May 19 01:42:16 1996
- mode: 0100666
- inode/dev: 0 2

fstat samples/sample.jpg
- size: 4762 bytes
- owner: 0 0
- created: Tue Sep 07 22:45:58 1999
- last accessed: Sun Sep 19 00:00:00 1999
- last modified: Sun May 19 01:42:16 1996
- mode: 0100666
- inode/dev: 0 0
```

Some fields don’t make sense on non-Unix platforms; for example, the (inode, dev) tuple provides a unique identity for each file on Unix, but can contain arbitrary data on other platforms.

The stat module contains a number of useful constants and helper functions for dealing with the members of the stat tuple. Some of these are shown in the examples below.

You can modify the mode and time fields using the chmod and utime functions:


###Using the os module to change a file’s privileges and timestamps

```
import os
import stat, time

infile = "samples/sample.jpg"
outfile = "out.jpg"

# copy contents
fi = open(infile, "rb")
fo = open(outfile, "wb")

while 1:
    s = fi.read(10000)
    if not s:
        break
    fo.write(s)

fi.close()
fo.close()

# copy mode and timestamp
st = os.stat(infile)
os.chmod(outfile, stat.S_IMODE(st[stat.ST_MODE]))
os.utime(outfile, (st[stat.ST_ATIME], st[stat.ST_MTIME]))

print "original", "=>"
print "mode", oct(stat.S_IMODE(st[stat.ST_MODE]))
print "atime", time.ctime(st[stat.ST_ATIME])
print "mtime", time.ctime(st[stat.ST_MTIME])

print "copy", "=>"
st = os.stat(outfile)
print "mode", oct(stat.S_IMODE(st[stat.ST_MODE]))
print "atime", time.ctime(st[stat.ST_ATIME])
print "mtime", time.ctime(st[stat.ST_MTIME])
```

```
original =>
mode 0666
atime Thu Oct 14 15:15:50 1999
mtime Mon Nov 13 15:42:36 1995
copy =>
mode 0666
atime Thu Oct 14 15:15:50 1999
mtime Mon Nov 13 15:42:36 1995
```

###Working with processes

The system function runs a new command under the current process, and waits for it to finish.

###Using the os module to run an operating system command
```
import os

if os.name == "nt":
    command = "dir"
else:
    command = "ls -l"

os.system(command)
```
```
-rwxrw-r--   1 effbot  effbot        76 Oct  9 14:17 README
-rwxrw-r--   1 effbot  effbot      1727 Oct  7 19:00 SimpleAsyncHTTP.py
-rwxrw-r--   1 effbot  effbot       314 Oct  7 20:29 aifc-example-1.py
-rwxrw-r--   1 effbot  effbot       259 Oct  7 20:38 anydbm-example-1.py
...
```

The command is run via the operating system’s standard shell, and returns the shell’s exit status. Under Windows 95/98, the shell is usually command.com whose exit status is always 0.

Warning: Since os.system passes the command on to the shell as is, it can be dangerous to use if you don’t check the arguments carefully (consider running os.system(“viewer %s” % file) with the file variable set to “sample.jpg; rm -rf $HOME”). When unsure, it’s usually better to use exec or spawn instead (see below).

The exec function starts a new process, replacing the current one (“go to process”, in other words). In the following example, note that the “goodbye” message is never printed:


###Using the os module to start a new process
```
import os
import sys

program = "python"
arguments = ["hello.py"]

print os.execvp(program, (program,) +  tuple(arguments))
print "goodbye"
```
```
hello again, and welcome to the show
```

Python provides a whole bunch of exec functions, with slightly varying behavior. The above example uses execvp, which searches for the program along the standard path, passes the contents of the second argument tuple as individual arguments to that program, and runs it with the current set of environment variables. See the Python Library Reference for more information on the other seven ways to call this function.

Under Unix, you can call other programs from the current one by combining exec with two other functions, fork and wait. The former makes a copy of the current process, the latter waits for a child process to finish.

###Using the os module to run another program (Unix)

```
import os
import sys

def run(program, *args):
    pid = os.fork()
    if not pid:
        os.execvp(program, (program,) +  args)
    return os.wait()[0]

run("python", "hello.py")

print "goodbye"
```
```
hello again, and welcome to the show
goodbye
```

The fork returns zero in the new process (the return from fork is the first thing that happens in that process!), and a non-zero process identifier in the original process. Or in other words, “not pid” is true only if we’re in the new process.

fork and wait are not available on Windows, but you can use the spawn function instead. Unfortunately, there’s no standard version of spawn that searches for an executable along the path, so you have to do that yourself:

###Using the os module to run another program (Windows)
```
import os
import string

def run(program, *args):
    # find executable
    for path in string.split(os.environ["PATH"], os.pathsep):
        file = os.path.join(path, program) + ".exe"
        try:
            return os.spawnv(os.P_WAIT, file, (file,) + args)
        except os.error:
            pass
    raise os.error, "cannot find executable"

run("python", "hello.py")

print "goodbye"
```
```
hello again, and welcome to the show
goodbye
```

You can also use spawn to run other programs in the background. The following example adds an optional mode argument to the run function; when set to os.P_NOWAIT, the script doesn’t wait for the other program to finish.

The default flag value os.P_WAIT tells spawn to wait until the new process is finished. Other flags include os.P_OVERLAY which makes spawn behave like exec, and os.P_DETACH which runs the new process in the background, detached from both console and keyboard.

###Using the os module to run another program in the background (Windows)

```
import os
import string

def run(program, *args, **kw):
    # find executable
    mode = kw.get("mode", os.P_WAIT)
    for path in string.split(os.environ["PATH"], os.pathsep):
        file = os.path.join(path, program) + ".exe"
        try:
            return os.spawnv(mode, file, (file,) + args)
        except os.error:
            pass
    raise os.error, "cannot find executable"

run("python", "hello.py", mode=os.P_NOWAIT)
print "goodbye"
```
```
goodbye
hello again, and welcome to the show
```

The following example provides a spawn method that works on either platform:

###Using either spawn or fork/exec to run another program

```
import os
import string

if os.name in ("nt", "dos"):
    exefile = ".exe"
else:
    exefile = ""

def spawn(program, *args):
    try:
        # check if the os module provides a shortcut
        return os.spawnvp(program, (program,) + args)
    except AttributeError:
        pass
    try:
        spawnv = os.spawnv
    except AttributeError:
        # assume it's unix
        pid = os.fork()
        if not pid:
            os.execvp(program, (program,) + args)
        return os.wait()[0]
    else:
        # got spawnv but no spawnp: go look for an executable
        for path in string.split(os.environ["PATH"], os.pathsep):
            file = os.path.join(path, program) + exefile
            try:
                return spawnv(os.P_WAIT, file, (file,) + args)
            except os.error:
                pass
        raise IOError, "cannot find executable"

#
# try it out!

spawn("python", "hello.py")

print "goodbye"
```
```
hello again, and welcome to the show
goodbye
```

The above example first attempts to call a function named spawnvp. If that doesn’t exist (it doesn’t, in 2.0 and earlier), the function looks for a function named spawnv and searches the path all by itself. As a last resort, it falls back on exec and fork.

###Working with daemon processes

On Unix, fork can also be used to turn the current process into a background process (a “daemon”). Basically, all you need to do is to fork off a copy of the current process, and terminate the original process:


###Example: Using the os module to run as daemon (Unix)
```
import os
import time

pid = os.fork()
if pid:
    os._exit(0) # kill original

print "daemon started"
time.sleep(10)
print "daemon terminated"
```

However, it takes a bit more work to create a real daemon. First, call setpgrp to make the new process a “process group leader”. Otherwise, signals sent to a (by that time) unrelated process group might cause problems in your daemon:

`os.setpgrp()`

It’s also a good idea to remove the user mode mask, to make sure files created by the daemon actually gets the mode flags specified by the program:

`os.umask(0)`

Then, you should redirect the stdout/stderr files, instead of just closing them. If you don’t do this, you may get unexpected exceptions the day some of your code tries to write something to the console via stdout or stderr.