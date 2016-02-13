# glob – Filename pattern matching

Even though the glob API is very simple, the module packs a lot of power. It is useful in any situation where your program needs to look for a list of files on the filesystem with names matching a pattern. If you need a list of filenames that all have a certain extension, prefix, or any common string in the middle, use glob instead of writing code to scan the directory contents yourself.

The pattern rules for glob are not regular expressions. Instead, they follow standard Unix path expansion rules. There are only a few special characters: two different wild-cards, and character ranges are supported. The patterns rules are applied to segments of the filename (stopping at the path separator, /). Paths in the pattern can be relative or absolute. Shell variable names and tilde (~) are not expanded.

###Example Data

The examples below assume the following test files are present in the current working directory:
```
$ python glob_maketestdata.py

dir
dir/file.txt
dir/file1.txt
dir/file2.txt
dir/filea.txt
dir/fileb.txt
dir/subdir
dir/subdir/subfile.txt
```

Note Use glob_maketestdata.py in the sample code to create these files if you want to run the examples.

###Wildcards

An asterisk (*) matches zero or more characters in a segment of a name. For example, dir/*.

import glob
for name in glob.glob('dir/*'):
    print name
The pattern matches every pathname (file or directory) in the directory dir, without recursing further into subdirectories.

$ python glob_asterisk.py

dir/file.txt
dir/file1.txt
dir/file2.txt
dir/filea.txt
dir/fileb.txt
dir/subdir
To list files in a subdirectory, you must include the subdirectory in the pattern:

import glob

print 'Named explicitly:'
for name in glob.glob('dir/subdir/*'):
    print '\t', name

print 'Named with wildcard:'
for name in glob.glob('dir/*/*'):
    print '\t', name
The first case above lists the subdirectory name explicitly, while the second case depends on a wildcard to find the directory.

$ python glob_subdir.py

Named explicitly:
        dir/subdir/subfile.txt
Named with wildcard:
        dir/subdir/subfile.txt
The results, in this case, are the same. If there was another subdirectory, the wildcard would match both subdirectories and include the filenames from both.

