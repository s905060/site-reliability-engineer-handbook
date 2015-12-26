# Simple Docker

###History

Old way

```
server : application 1 1
```

Current way:
```
OS    OS    OS     <== consumes resources (RAM, HD, CPU) before even an app installed /licensing  | better security
VM1 VM2 VM2..   x 10
<— Hypervisor —>
Physical Machine x 1
```