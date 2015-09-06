# Shell command to bulk change file extensions in a directory (Linux)

This article will explain how to change the file extension for all files in a directory in Linux using a simple bash shell command.

1. Change from one extension to another

The command below will rename all files with the extension .php4 to .php
```
for f in *.php4; do mv $f `basename $f .php4`.php; done;
```

2. Add (append) an extension to all files

The command below add the extension .txt to all files in the directory
```
for f in *; do mv $f `basename $f `.txt; done;
```

2. Remove (delete) an extension from all files

The command below remove the extension .txt from all files in the directory
```
for f in *.txt; do mv $f `basename $f .txt`; done;
```
