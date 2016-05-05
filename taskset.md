# taskset


## How to run program or process on specific CPU cores on Linux

As multi-core CPUs become increasingly popular on server-grade hardware as well as end-user desktop PCs or laptops, there have been growing efforts in the community (e.g., in terms of programming models, compiler or operating system support) towards developing applications optimized for multi-core architecture.

One operating system (OS) support often exploited to run performance-critical applications on multi-core processors is so-called "processor affinity" or "CPU pinning". This is an OS-specific feature that "binds" a running process or program to particular CPU core(s).

Binding a program to specific CPU cores can be beneficial in several scenarios. For example, when an application with highly cache-bound workload runs together with other CPU-intensive jobs, pinning the application to a specific CPU would reduce CPU cache misses. Also, when two processes communicate via shared memory intensively, scheduling both processes on the cores in the same NUMA domain would speed up their performance.

In this tutorial, I will describe how to run a program or process on specific CPU cores on Linux.

To assign particular CPU cores to a program or process, you can use taskset, a command line tool for retrieving or setting a process' CPU affinity on Linux.


## Install taskset on Linux

The taskset tool is part of "util-linux" package in Linux, and most Linux distros come with the package pre-installed by default. If taskset is not available on your Linux system, install it as follows.

To install taskset on Debian, Ubuntu or Linux Mint:

```bash
$ sudo apt-get install util-linux
```

To install taskset on Fedora, CentOS or RHEL:
```bash
$ sudo yum install util-linux
```