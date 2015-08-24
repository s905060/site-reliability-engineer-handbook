# SED

When you copy a DOS file to Unix, you could find \r\n in the end of each line. This example converts the DOS file format to Unix file format using sed command.


Syntax:
• s is substitute command
• / is a delimiter
• REGEXP is regular expression to match
• REPLACEMENT is a value to replace
FLAGS can be any of the following :
• g Replace all the instance of REGEXP with REPLACEMENT
• n Could be any number,replace nth instance of the REGEXP with REPLACEMENT.
• p If substitution was made, then prints the new pattern space.
• i match REGEXP in a case-insensitive manner.
• w file If substitution was made, write out the result to the given file.


```
$sed 's/.$//' filename
```

Print file content in reverse order

```
$ sed -n '1!G;h;$p' thegeekstuff.txt
```

Add line number for all non-empty-lines in a file

```
$ sed '/./=' thegeekstuff.txt | sed 'N; s/\n/ /'
```