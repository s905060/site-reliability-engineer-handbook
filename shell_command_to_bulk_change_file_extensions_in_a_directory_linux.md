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

OR


This will do the job for you.
```
rename 's/.m4b$/.m4a/' *.m4b
```

For a test run you can use this command:
```
rename 's/.m4b$/.m4a/' *.m4b -vn
```

-v means "verbose" and it will output the names of the files when it renames them.

-n will do a test run where it won't rename any files, But will show you a list of files that would be renamed.


Straight from Greg's Wiki:

# Rename all *.txt to *.text
```
for f in *.txt; do 
mv -- "$f" "${f%.txt}.text"
done
```
Also see the entry on why you shouldn't parse ls.

Edit: if you have to use basename your syntax would be:
```
for f in *.txt; do
mv "$f" "$(basename "$f" .txt).text"
done
```