# [How do you read a segfault kernel log message](http://stackoverflow.com/questions/2179403/how-do-you-read-a-segfault-kernel-log-message)

```
segfault at 10 ip 00007f9bebcca90d sp 00007fffb62705f0 error 4 in libQtWebKit.so.4.5.2[7f9beb83a000+f6f000]
segfault at 10 ip 00007fa44d78890d sp 00007fff43f6b720 error 4 in libQtWebKit.so.4.5.2[7fa44d2f8000+f6f000]
segfault at 11 ip 00007f2b0022acee sp 00007fff368ea610 error 4 in libQtWebKit.so.4.5.2[7f2aff9f7000+f6f000]
segfault at 11 ip 00007f24b21adcee sp 00007fff7379ded0 error 4 in libQtWebKit.so.4.5.2[7f24b197a000+f6f000]
```

This is a segfault due to following a null pointer trying to find code to run \(that is, during an instruction fetch\).

## If this were a program, not a shared library

Run `addr2line -e yourSegfaultingProgram 00007f9bebcca90d` \(and repeat for the other instruction pointer values given\) to see where the error is happening. Better, get a debug-instrumented build, and reproduce the problem under a debugger such as gdb.

## Since it's a shared library

You're hosed, unfortunately; it's not possible to know where the libraries were placed in memory by the dynamic linker after-the-fact. Reproduce the problem under gdb.

## What the error means

Here's the breakdown of the fields:

* **address** \(after the **at**\) - the location in memory the code is trying to access \(it's likely that **10** and **11** are offsets from a pointer we expect to be set to a valid value but which is instead pointing to **0**\)

* **ip** - instruction pointer, ie. where the code which is trying to do this lives

* **sp** - stack pointer

* **error** - An error code for page faults; see below for what this means on x86.

```
/*
* Page fault error code bits:
*
*   bit 0 ==    0: no page found       1: protection fault
*   bit 1 ==    0: read access         1: write access
*   bit 2 ==    0: kernel-mode access  1: user-mode access
*   bit 3 ==                           1: use of reserved bit detected
*   bit 4 ==                           1: fault was an instruction fetch
*/
```



