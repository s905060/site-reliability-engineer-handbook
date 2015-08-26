# Exit codes

Exit codes

Every program you start terminates with an exit code and reports it to the operating system. This exit code can be utilized by Bash. You can show it, you can act on it, you can control script flow with it. The code is a number between 0 and 255. Values from 126 to 255 are reserved for use by the shell directly, or for special purposes, like reporting a termination by a signal:
```
126: the requested command (file) was found, but can't be executed
127: command (file) not found
128: according to ABS it's used to report an invalid argument to the exit builtin, but I wasn't able to verify that in the source code of Bash (see code 255)
128 + N: the shell was terminated by the signal N
255: wrong argument to the exit builtin (see code 128)
```

**The lower codes 0 to 125 are not reserved and may be used for whatever the program likes to report. A value of 0 means successful termination, a value not 0 means unsuccessful termination. This behavior (== 0, != 0) is also what Bash reacts to in some flow control statements.**

An example of using the exit code of the program grep to check if a specific user is present in /etc/passwd:
```
if grep ^root /etc/passwd; then
   echo "The user root was found"
else
   echo "The user root was not found"
fi
```
A common decision making command is "test" or its equivalent "[". But note that, when calling test with the name "[", the square brackets are not part of the shell syntax, the left bracket is the test command!
```
if [ "$mystring" = "Hello world" ]; then
   echo "Yeah dude, you entered the right words..."
else
   echo "Eeeek - go away..."
fi
```
Read more about the test command
A common exit code check method uses the "||" or "&&" operators. This lets you execute a command based on whether or not the previous command completed successfully:

grep ^root: /etc/passwd >/dev/null || echo "root was not found - check the pub at the corner." which vi && echo "Your favorite editor is installed."

Please, when your script exits on errors, provide a "FALSE" exit code, so others can check the script execution.