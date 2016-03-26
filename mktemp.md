# mktemp

Use any one of the following command to create temporary empty file names. The first command is special as it use the redirection operator >, the redirection refers to the standard output. So you are creating a new file or destroying existing file:
```bash
>/tmp/filename
```
OR
```bash
echo -n "" > /tmp/filename
```
The touch command can be also used to create temporary empty file names:
```bash
touch /tmp/newfilename
```


### mktemp Command


To make temporary unique filename use the mktemp command. In this example, create temporary filename in using a user’s $TMPDIR environment variable:
```bash
mktemp
```

Sample outputs:
```bash
/tmp/tmp.yTfJX35144
```
Use /tmp/tmp.yTfJX35144 to store your output. You can store filename to a variable:
```bash
OUT="$(mktemp)"
ls > $OUT
```

The following bash scripting illustrates a simple use of mktemp where the script should quit if it cannot get a safe temporary file
```bash
#!/bin/bash
OUT=$(mktemp /tmp/output.XXXXXXXXXX) || { echo "Failed to create temp file"; exit 1; }
echo "Today is $(date)"  >> $OUT
```

The mktemp utility takes the given filename template and overwrites a portion of it to create a unique filename. The template may be any filename with some number of ‘Xs’ appended to it, for example /tmp/tfile.XXXXXXXXXX.


### TMPDIR Environment Variable

By default mktemp will use user’s $TMPDIR. If not defined it will use /tmp. You can use the specified directory as a prefix when generating the temporary filename. The directory will be overridden by the user’s TMPDIR environment variable if it is set. In this example the temporary file will be created in /chroot/apache/var/tmp unless the user’s TMPDIR environment variable specifies otherwise:
```bash
mktemp -p /chroot/apache/var/tmp php.lock.XXXXXXXXXX
```