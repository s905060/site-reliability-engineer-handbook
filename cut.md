# Cut

Display the 1st field (employee name) from a colon delimited file
```
$ sort namesd.txt | uniq –cd
      2 Alex Jason:200:Sales
      2 Emma Thomas:100:Marketing
$ cut -d: -f 1 names.txt
Emma Thomas
Alex Jason
Madison Randy
Sanjay Gupta
Nisha Singh
```

Display 1st and 3rd field from a colon delimited file
```
$ cut -d: -f 1,3 names.txt
Emma Thomas:Marketing
Alex Jason:Sales
Madison Randy:Product Development
Sanjay Gupta:Support
Nisha Singh:Sales
```
Display only the first 8 characters of every line in a file
```
$ cut -c 1-8 names.txt
Emma Tho
Alex Jas
Madison
Sanjay G
Nisha Si
```
Misc Cut command examples
```
• cut -d: -f1 /etc/passwd Displays the unix login names for all the users in the system.
• free|tr-s''|sed'/^Mem/!d'|cut-d""-f2Displaysthe total memory available on the system.
```