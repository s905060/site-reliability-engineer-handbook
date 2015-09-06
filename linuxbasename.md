# Basename

basename will print NAME with any leading directory components removed. If specified, it will also remove a trailing SUFFIX (typically a file extention).

Examples

Get the name of the home folder:
```
$ basename ~
```

Extract the file name from the variable pathnamevar and store in the variable result using parameter expansion $( )
```
$ result=$(basename "$pathnamevar")
```

A script to rename file extensions:
```
#BatchRenameExt
for file in *.$1; do
mv $file `basename $file $1`.$2
done

$ BatchRenameExt htm html
```