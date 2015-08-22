### grep command examples

Search for a given string in a file (case in-sensitive search).

```
$ grep -i "the" demo_file
```

Print the matched line, along with the 3 lines after it.

```
$ grep -A 3 -i "example" demo_text
```

Search for a given string in all files recursively

```
$ grep -r "ramesh" *
```