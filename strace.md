# Strace

### What is that process doing RIGHT NOW?
Ever had a process suddenly hog lots of CPU? Or had a process seem to be hanging?

Then you find the pid, and do this:
```
root@dev:~# strace -p 15427
Process 15427 attached - interrupt to quit
futex(0x402f4900, FUTEX_WAIT, 2, NULL 
Process 15427 detached
```

Ah. So in this case it's hanging in a call to futex(). Incidentally in this case it doesn't tell us all that much - hanging on a futex can be caused by a lot of things (a futex is a locking mechanism in the Linux kernel). The above is from a normally working but idle Apache child process that's just waiting to be handed a request.

But "strace -p" is highly useful because it removes a lot of guesswork, and often removes the need for restarting an app with more extensive logging (or even recompile it).

### What is taking time?
You can always recompile an app with profiling turned on, and for accurate information, especially about what parts of your own code that is taking time that is what you should do. But often it is tremendously useful to be able to just quickly attach strace to a process to see what it's currently spending time on, especially to diagnose problems. Is that 90% CPU use because it's actually doing real work, or is something spinning out of control.

Here's what you do:
```
root@dev:~# strace -c -p 11084
Process 11084 attached - interrupt to quit
Process 11084 detached
% time     seconds  usecs/call     calls    errors syscall
------ ----------- ----------- --------- --------- ----------------
 94.59    0.001014          48        21           select
  2.89    0.000031           1        21           getppid
  2.52    0.000027           1        21           time
------ ----------- ----------- --------- --------- ----------------
100.00    0.001072                    63           total
root@dev:~# 
```

After you've started strace with -c -p you just wait for as long as you care to, and then exit with ctrl-c. Strace will spit out profiling data as above.

In this case, it's an idle Postgres "postmaster" process that's spending most of it's time quietly waiting in select(). In this case it's calling getppid() and time() in between each select() call, which is a fairly standard event loop.

You can also run this "start to finish", here with "ls":
```
root@dev:~# strace -c >/dev/null ls
% time     seconds  usecs/call     calls    errors syscall
------ ----------- ----------- --------- --------- ----------------
 23.62    0.000205         103         2           getdents64
 18.78    0.000163          15        11         1 open
 15.09    0.000131          19         7           read
 12.79    0.000111           7        16           old_mmap
  7.03    0.000061           6        11           close
  4.84    0.000042          11         4           munmap
  4.84    0.000042          11         4           mmap2
  4.03    0.000035           6         6         6 access
  3.80    0.000033           3        11           fstat64
  1.38    0.000012           3         4           brk
  0.92    0.000008           3         3         3 ioctl
  0.69    0.000006           6         1           uname
  0.58    0.000005           5         1           set_thread_area
  0.35    0.000003           3         1           write
  0.35    0.000003           3         1           rt_sigaction
  0.35    0.000003           3         1           fcntl64
  0.23    0.000002           2         1           getrlimit
  0.23    0.000002           2         1           set_tid_address
  0.12    0.000001           1         1           rt_sigprocmask
------ ----------- ----------- --------- --------- ----------------
100.00    0.000868                    87        10 total
```

Pretty much what you'd expect, it spents most of it's time in two calls to read the directory entries (only two since it was run on a small directory).