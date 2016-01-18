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

