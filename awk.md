#AWK

To print the previous, the pattern matching line and next line: 
```
$ grep -C1 Solaris file
Linux
Solaris
AIX
```

-C is to print both lines above and below pattern.
```
$ awk '/Sola­ris­/{print x;prin­t;g­etl­ine­;pr­int­;ne­xt}­{x=­$0;}' file
Linux
Solaris
AIX
```

Remove duplicate lines using awk

```
$ awk '!($0 in array) { array[$0]; print }' temp
```

Print all lines from /etc/passwd that has the same uid and gid

```
$awk -F ':' '$3==$4' passwd.txt
```

Print only specific field from a file.

```
$ awk '{print $2,$5;}' employee.txt
```