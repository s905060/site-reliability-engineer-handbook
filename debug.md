# Debug

Shell script with Debug command inside:
Add set –xv inside the shell script now to debug the output as shown below.
```￼
$ ./filesize.sh
Total file size in current directory: 652
$ cat filesize.sh
#!/bin/bash
set -xv
for filesize in $(ls -l . | grep "^-" | awk '{print $5}')
do
  let totalsize=$totalsize+$filesize
done
echo "Total file size in current directory: $totalsize"
```

Output of Shell script with Debug command inside:
```
$ ./fs.sh
++ ls -l .
++ grep '^-'
++ awk '{print $5}'
+ for filesize in '$(ls -l . | grep "^-" | awk '\''{print
$5}'\'')'
+ let totalsize=+178
+ for filesize in '$(ls -l . | grep "^-" | awk '\''{print
$5}'\'')'
+ let totalsize=178+285
+ for filesize in '$(ls -l . | grep "^-" | awk '\''{print
$5}'\'')'
+ let totalsize=463+189
+ echo 'Total file size in current directory: 652'
Total file size in current directory: 652
```

Execute Shell script with debug option:
Instead of giving the set –xv inside the shell script, you can also provide that while executing the shell script as shown below.
```
$ bash -xv filesize.sh
```