# SED

When you copy a DOS file to Unix, you could find \r\n in the end of each line. This example converts the DOS file format to Unix file format using sed command.


Syntax:
* s is substitute command
* / is a delimiter
* REGEXP is regular expression to match
* REPLACEMENT is a value to replace
FLAGS can be any of the following :
* g Replace all the instance of REGEXP with REPLACEMENT
* n Could be any number,replace nth instance of the REGEXP with REPLACEMENT.
* p If substitution was made, then prints the new pattern space.
* i match REGEXP in a case-insensitive manner.
* w file If substitution was made, write out the result to the given file.
* We can use different delimiters ( one of @ % ; : ) instead of /


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

Substitute all Appearances of a Word Using sed s//g
The below sed command replaces all occurrences of Linux to Linux-Unix using global substitution flag “g”.
```
$ sed 's/Linux/Linux-Unix/g' thegeekstuff.txt

# Instruction Guides
1. Linux-Unix Sysadmin, Linux-Unix Scripting etc.
2. Databases - Oracle, mySQL etc.
3. Security (Firewall, Network, Online Security etc)
4. Storage in Linux-Unix
5. Productivity (Too many technologies to explore, not
much time available)
#  Additional FAQS
6. Windows- Sysadmin, reboot etc.
```