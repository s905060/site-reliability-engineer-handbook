# SED

When you copy a DOS file to Unix, you could find \r\n in the end of each line. This example converts the DOS file format to Unix file format using sed command.

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