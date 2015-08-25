# Nice

Kernel decides how much processor time is required for a process based on the nice value. Possible nice value range is: -20 to 20. A process that has a nice value of -20 is very high priority. The process that has a nice value of 20 is very low priority.
Use ps axl to display the nice value of all running process as shown below.

Note: Only root user can set a negative nice value. Login as root and try the same. Please note that there is a double dash before the 10 in the nice command below.

```
# nice --10 ./nice-test.sh &
[1] 13060
# ps axl | grep nice-test
4     0 13060 13024  10 -10  5388  964 wait   S<   pts/1
0:00 /bin/bash ./nice-test.sh
[Note: 6th column with value -10 is the nice value of the shell-script.]
```