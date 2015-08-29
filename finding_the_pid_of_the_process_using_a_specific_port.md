# Finding the PID of the process using a specific port?

How do I find out running processes were associated with each open port? How do I find out what process has open tcp port 111 or udp port 7000 under Linux?

You can the following programs to find out about port numbers and its associated process:

1. **netstat** - a command-line tool that displays network connections, routing tables, and a number of network interface statistics.
2. **fuser** - a command line tool to identify processes using files or sockets.
3. **lsof** - a command line tool to list open files under Linux / UNIX to report a list of all open files and the processes that opened them.
4. **/proc/$pid/** file system - Under Linux /proc includes a directory for each running process (including kernel processes) at /proc/PID, containing information about that process, notably including the processes name that opened port.

You must run above command(s) as the root user.



```
$ sudo netstat -nlp | grep 80
tcp  0  0  0.0.0.0:80  0.0.0.0:*  LISTEN  125004/nginx
```

```
# lsof -i :25
COMMAND  PID        USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
exim4   2799 Debian-exim    3u  IPv4   6645      0t0  TCP localhost:smtp (LISTEN)
exim4   2799 Debian-exim    4u  IPv6   6646      0t0  TCP localhost:smtp (LISTEN)
```

```
lsof -i :portNumber
lsof -i tcp:portNumber
lsof -i udp:portNumber
lsof -i :80
lsof -i :80 | grep LISTEN
```

### fuser command

Find out the processes PID that opened tcp port 7000, enter:
```
# fuser 7000/tcp
```

Sample outputs:
```
7000/tcp:             3813
```
Finally, find out process name associated with PID # 3813, enter:
```
# ls -l /proc/3813/exe
```
Sample outputs:
```
lrwxrwxrwx 1 vivek vivek 0 2010-10-29 11:00 /proc/3813/exe -> /usr/bin/transmission
```
/usr/bin/transmission is a bittorrent client, enter:
```
# man transmission
```
OR
```
# whatis transmission
```
Sample outputs:
```
transmission (1)     - a bittorrent client
```