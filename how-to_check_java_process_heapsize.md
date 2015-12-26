# How-to check Java process heapsize

In Java startup set heapsize, but we are not sure whether the entry into force. You can get the size heapsize by jstat, some small script below, PID for the process java process id

```
jstat -gccapacity PID |  awk '{print ($3+$9+$14)/1024}'  | tail -1
```

Unit display in MB