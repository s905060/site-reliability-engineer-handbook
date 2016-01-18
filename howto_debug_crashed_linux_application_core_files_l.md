# HowTo: Debug Crashed Linux Application Core Files Like A Pro

>A core dump is the recorded state of the working memory of a computer program at a specific time, generally when the program has terminated abnormally (crashed). In practice, other key pieces of program state are usually dumped at the same time, including the processor registers, which may include the program counter and stack pointer, memory management information, and other processor and operating system flags and information. The name comes from the once-standard memory technology core memory. Core dumps are often used to diagnose or debug errors in computer programs.

>On many operating systems, a fatal error in a program automatically triggers a core dump, and by extension the phrase "to dump core" has come to mean, in many cases, any fatal error, regardless of whether a record of the program memory is created.

Core dumps are often used to diagnose or debug errors in Linux or UNIX programs. Core dumps can serve as useful debugging aids for sys admins to find out why Application like Lighttpd, Apache, PHP-CGI or any other program crashed. Many vendors and open source project author requests a core file to troubleshoot a program. A core file is generated when an application program abnormally terminates due to bug, operating system security protection schema, or program simply try to write beyond the area of memory it has allocated, and so on. This article explains how to turn on core file support and track down bugs in programs.

###Turn On Core File Creation Support

By default most Linux distributions turn off core file creation (at least this is true for RHEL, CentOS, Fedora and Suse Linux). You need to use the ulimit command to configure core files.

**See The Current Core File Limits**
Type the following command:
```
# ulimit -c
```

Sample outputs:
```
0
```

The output 0 (zero) means core file is not created.

**Change Core File Limits**

In this example, set the size limit of core files to 75000 bytes:
```
# ulimit -c 75000
```

###HowTo: Enable Core File Dumps For Application Crashes And Segmentation Faults

Edit /etc/profile file and find line that read as follows to make persistent configuration:
```
ulimit -S -c 0 > /dev/null 2>&1
```

Update it as follows:
```
ulimit -c unlimited >/dev/null 2>&1
```

Save and close the file. Edit /etc/sysctl.conf, enter:
```
# vi /etc/sysctl.conf
```

Append the following lines:
```
kernel.core_uses_pid = 1
kernel.core_pattern = /tmp/core-%e-%s-%u-%g-%p-%t
fs.suid_dumpable = 2
```

Save and close the file. Where,

1. **kernel.core_uses_pid = 1** - Appends the coring processes PID to the core file name.

2. **fs.suid_dumpable = 2** - Make sure you get core dumps for setuid programs.

3. **kernel.core_pattern = /tmp/core-%e-%s-%u-%g-%p-%t** - When the application terminates abnormally, a core file should appear in the /tmp. The kernel.core_pattern sysctl controls exact location of core file. You can define the core file name with the following template whih can contain % specifiers which are substituted by the following values when a core file is created:

    * **%% -** A single % character
    * **%p -** PID of dumped process
    * **%u -** real UID of dumped process
    * **%g -** real GID of dumped process
    * ** %s -** number of signal causing dump
    * **%t -** time of dump (seconds since 0:00h, 1 Jan 1970)
    * **%h -** hostname (same as ’nodename’ returned by uname(2))
    * **%e -** executable filename

Finally, enable debugging for all apps, enter (Redhat and friends specific):
```
# echo "DAEMON_COREFILE_LIMIT='unlimited'" >> /etc/sysconfig/init
```

Reload the settings in /etc/sysctl.conf by running the following command:
```
# sysctl -p
```

###How Do I Enable Core Dumping For Specific Deamon?

To enable core dumping for specific deamons, add the following line in the /etc/sysconfig/daemon-file file. In this example, edit /etc/init.d/lighttped and add line as follows:
```
DAEMON_COREFILE_LIMIT='unlimited'
```

Please note that DAEMON_COREFILE_LIMIT is Redhat specific, for all other distro add configuration as follows:
```
ulimit -c unlimited >/dev/null 2>&1
echo /tmp/core-%e-%s-%u-%g-%p-%t > /proc/sys/kernel/core_pattern
```

Save and close the file. Restart / reload lighttpd:
```
# /etc/init.d/lighttpd restart
# su - lighttpd
$ ulimit -c
```

Sample outputs:
```
unlimited
```

Now, you can send core files to vendor or software writes.

###How Do I Read Core Files?

You need use the gdb command as follows:
```
$ gdb /path/to/application /path/to/corefile
```