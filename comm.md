# Comm

The comm command is a utility that is used to compare two files for common and distinct lines.
comm reads two files as input, regarded as lines of text. comm outputs one file, which contains three columns. The first two columns contain lines unique to the first and second file, respectively. The last column contains lines common to both.

Like uniq, comm expects that the lines are sorted, so also with this command we’ll use the command sort.

From COMM(1) man page, the options available are:

* -1 suppress lines unique to FILE1
* -2 suppress lines unique to FILE2
* -3 suppress lines that appear in both files

So if we have as Input files:

```
# cat file.txt
aa bb cc
aa bb
aa
cc bb
fe fe fe
aa bb fe
aa bb
cc bb
 
# cat file2.txt
 
aa bb cc
aa bb dd
aa 22
cc bb 33
fe fe fe fe
aa bb fe
aa bb
cc bb 11
```

To find only those lines which are common to both the files
First we sort both files in a temporary file:
```
#sort file.txt > file.txt.sorted
#sort file2.txt > file2.txt.sorted
```

Now we can compare them:
```
#comm file.txt.sorted file2.txt.sorted
aa bb
aa bb cc
aa bb fe
```

With process substitution we can do all this with one line and get the same result:
```
# comm -12 < (sort file.txt) <(sort file2.txt)
```

Note without the -12 option we would get an output like this one:
```
# comm < (sort file.txt) <(sort file2.txt)
12 34
aa
aa 22
aa bb
aa bb
aa bb cc
aa bb dd
aa bb fe
cc bb
cc bb
cc bb 11
cc bb 33
fe fe fe
fe fe fe fe
```