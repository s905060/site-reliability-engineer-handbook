# Kill

kill command is a command that is used to send a signal in order to request the termination of the process. 

We typically use kill -SIGNAL PID, where you know the
PID of the process.

* The kill() system call can be used to send any signal to any process group or process.
* int kill( pid_t pid, int signo );

|pid |Meaning
| -- | --
| \> 0 |send signal to process pid
| == 0 |send signal to all processes whose process group ID equals the sender’s pgid. e.g. parent kills all children
| -1 |send signal to every process for which the calling process has permission to send signals