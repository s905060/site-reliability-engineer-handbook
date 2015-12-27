# getpass

Prompt the user for a password without echoing

Many programs that interact with the user via the terminal need to ask the user for password values without showing what the user types on the screen. The getpass module provides a portable way to handle such password prompts securely.

Example

The getpass() function prints a prompt then reads input from the user until they press return. The input is passed back as a string to the caller.
```
import getpass

p = getpass.getpass()
print 'You entered:', p
```

The default prompt, if none is specified by the caller, is “Password:”.
```
$ python getpass_defaults.py
Password:
You entered: sekret
```

The prompt can be changed to any value your program needs.
```
import getpass

p = getpass.getpass(prompt='What is your favorite color? ')
if p.lower() == 'blue':
    print 'Right.  Off you go.'
else:
    print 'Auuuuugh!'
```

I don’t recommend such an insecure authentication scheme, but it illustrates the point.
```
$ python getpass_prompt.py
What is your favorite color?
Right.  Off you go.
$ python getpass_prompt.py
What is your favorite color?
Auuuuugh!
```

By default, getpass() uses stdout to print the prompt string. For a program which may produce useful output on sys.stdout, it is frequently better to send the prompt to another stream such as sys.stderr.
```
import getpass
import sys

p = getpass.getpass(stream=sys.stderr)
print 'You entered:', p
```

This way standard output can be redirected (to a pipe or file) without seeing the password prompt. The value entered by the user is still not echoed back to the screen.
```
$ python getpass_stream.py >/dev/null
Password:
```

###Using getpass Without a Terminal

Under Unix, getpass() always requires a tty it can control via termios, so echo can be disabled. This means values will not be read from a non-terminal stream redirected to standard input.
```
$ echo "sekret" | python getpass_defaults.py
Traceback (most recent call last):
 File "getpass_defaults.py", line 34, in
   p = getpass.getpass()
 File "/Library/Frameworks/Python.framework/Versions/2.5/lib/python2.5/getpass.py", line 32, in unix_getpass
   old = termios.tcgetattr(fd)     # a copy to save
termios.error: (25, 'Inappropriate ioctl for device')
```

It is up to the caller to detect when the input stream is not a tty and use an alternate method for reading in that case.
```
import getpass
import sys

if sys.stdin.isatty():
    p = getpass.getpass('Using getpass: ')
else:
    print 'Using readline'
    p = sys.stdin.readline().rstrip()

print 'Read: ', p
```

With a tty:
```
$ python ./getpass_noterminal.py
Using getpass:
Read:  sekret
```

Without a tty:
```
$ echo "sekret" | python ./getpass_noterminal.py
Using readline
Read:  sekret
```