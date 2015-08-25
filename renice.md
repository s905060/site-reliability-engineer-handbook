# Renice

Renice alters the scheduling priority of a running process.

To increase the nice value (thus reducing the priority), execute the renice command as shown below.
￼```
$ ps axl | grep nice-test
0 509 13245 13216 30 10 5244 968 wait SN pts/1 0:00 /bin/bash ./nice-test.sh
$ renice 16 -p 13245
13245: old priority 10, new priority 16
$ ps axl | grep nice-test
0 509 13245 13216 36 16 5244 968 wait SN pts/1 0:00 /bin/bash ./nice-test.sh
```

[Note: Now, the 6th column of the nice-test.sh (PID 13245) shows the new nice value of 16.]