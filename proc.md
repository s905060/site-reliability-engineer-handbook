# More about /proc/PID/file & procfs File System

/proc (or procfs) is a pseudo-file system that it is dynamically generated after each reboot. It is used to access kernel information. procfs is also used by Solaris, BSD, AIX and other UNIX like operating systems. Now, you know how many file descriptors are being used by a process. You will find more interesting stuff in /proc/$PID/file directory:

/proc/PID/cmdline : process arguments
/proc/PID/cwd : process current working directory (symlink)
/proc/PID/exe : path to actual process executable file (symlink)
/proc/PID/environ : environment used by process
/proc/PID/root : the root path as seen by the process. For most processes this will be a link to / unless the process is running in a chroot jail.
/proc/PID/status : basic information about a process including its run state and memory usage.
/proc/PID/task : hard links to any tasks that have been started by this (the parent) process.