# Lsof

lsof stands for List Open Files.

It is easy to remember lsof command if you think of it as “ls + of”, where ls stands for list, and of stands for open files.

It is a command line utility which is used to list the information about the files that are opened by various processes. In unix, everything is a file, ( pipes, sockets, directories, devices, etc.). So by using lsof, you can get the information about any opened files.

### Introduction to lsof

Simply typing lsof will provide a list of all open files belonging to all active processes.

```
# lsof

COMMAND  PID       USER   FD      TYPE     DEVICE  SIZE/OFF       NODE NAME
init       1       root  cwd       DIR        8,1      4096          2 /
init       1       root  txt       REG        8,1    124704     917562 /sbin/init
init       1       root    0u      CHR        1,3       0t0       4369 /dev/null
init       1       root    1u      CHR        1,3       0t0       4369 /dev/null
init       1       root    2u      CHR        1,3       0t0       4369 /dev/null
init       1       root    3r     FIFO        0,8       0t0       6323 pipe
...
```

By default One file per line is displayed. Most of the columns are self explanatory. We will explain the details about couple of cryptic columns (FD and TYPE).

FD – Represents the file descriptor. Some of the values of FDs are,

* cwd – Current Working Directory
* txt – Text file
* mem – Memory mapped file
* mmap – Memory mapped device
* NUMBER – Represent the actual file descriptor. The 
character after the number i.e ‘1u’, represents the mode in which the file is opened. r for read, w for write, u for read and write.
TYPE – Specifies the type of the file. Some of the values of TYPEs are,
* REG – Regular File
* DIR – Directory
* FIFO – First In First Out
* CHR – Character special file
* 

For a complete list of FD & TYPE, refer man lsof.

### List processes which opened a specific file

You can list only the processes which opened a specific file, by providing the filename as arguments.

```
# lsof /var/log/syslog

COMMAND  PID   USER   FD   TYPE DEVICE SIZE/OFF   NODE NAME
rsyslogd 488 syslog    1w   REG    8,1     1151 268940 /var/log/syslog
```

###  List opened files under a directory

You can list the processes which opened files under a specified directory using ‘+D’ option. +D will recurse the sub directories also. If you don’t want lsof to recurse, then use ‘+d’ option.

```
# lsof +D /var/log/

COMMAND   PID   USER  FD   TYPE DEVICE SIZE/OFF   NODE NAME
rsyslogd  488 syslog   1w   REG    8,1     1151 268940 /var/log/syslog
rsyslogd  488 syslog   2w   REG    8,1     2405 269616 /var/log/auth.log
console-k 144   root   9w   REG    8,1    10871 269369 /var/log/ConsoleKit/history
```

###  List opened files based on process names starting with

You can list the files opened by process names starting with a string, using ‘-c’ option. -c followed by the process name will list the files opened by the process starting with that processes name. You can give multiple -c switch on a single command line.

```
# lsof -c ssh -c init

COMMAND    PID   USER   FD   TYPE DEVICE SIZE/OFF   NODE NAME
init         1       root  txt    REG        8,1   124704  917562 /sbin/init
init         1       root  mem    REG        8,1  1434180 1442625 /lib/i386-linux-gnu/libc-2.13.so
init         1       root  mem    REG        8,1    30684 1442694 /lib/i386-linux-gnu/librt-2.13.so
...
ssh-agent 1528 lakshmanan    1u   CHR        1,3      0t0    4369 /dev/null
ssh-agent 1528 lakshmanan    2u   CHR        1,3      0t0    4369 /dev/null
ssh-agent 1528 lakshmanan    3u  unix 0xdf70e240      0t0   10464 /tmp/ssh-sUymKXxw1495/agent.1495
```

###  List processes using a mount point

Sometime when we try to umount a directory, the system will say “Device or Resource Busy” error. So we need to find out what are all the processes using the mount point and kill those processes to umount the directory. By using lsof we can find those processes.

```
# lsof /home
```

The following will also work.
```
# lsof +D /home/
```

###  List files opened by a specific user

In order to find the list of files opened by a specific users, use ‘-u’ option.

```
# lsof -u lakshmanan

COMMAND    PID       USER   FD   TYPE     DEVICE SIZE/OFF       NODE NAME
update-no 1892 lakshmanan   20r  FIFO        0,8      0t0      14536 pipe
update-no 1892 lakshmanan   21w  FIFO        0,8      0t0      14536 pipe
bash      1995 lakshmanan  cwd    DIR        8,1     4096     393218 /home/lakshmanan
Sometimes you may want to list files opened by all users, expect some 1 or 2. In that case you can use the ‘^’ to exclude only the particular user as follows
```
```
# lsof -u ^lakshmanan

COMMAND    PID       USER   FD      TYPE     DEVICE  SIZE/OFF       NODE NAME
rtkit-dae 1380      rtkit    7u     0000        0,9         0       4360 anon_inode
udisks-da 1584       root  cwd       DIR        8,1      4096          2 /
```

The above command listed all the files opened by all users, expect user ‘lakshmanan’.

###  List all open files by a specific process

You can list all the files opened by a specific process using ‘-p’ option. It will be helpful sometimes to get more information about a specific process.

```
# lsof -p 1753

COMMAND  PID       USER   FD   TYPE DEVICE SIZE/OFF    NODE NAME
bash    1753 lakshmanan  cwd    DIR    8,1     4096  393571 /home/lakshmanan/test.txt
bash    1753 lakshmanan  rtd    DIR    8,1     4096       2 /
bash    1753 lakshmanan  255u   CHR  136,0      0t0       3 /dev/pts/0
...
```

###  Kill all process that belongs to a particular user

When you want to kill all the processes which has files opened by a specific user, you can use ‘-t’ option to list output only the process id of the process, and pass it to kill as follows

```
# kill -9 `lsof -t -u lakshmanan`
```

The above command will kill all process belonging to user ‘lakshmanan’, which has files opened.

Similarly you can also use ‘-t’ in many ways. For example, to list process id of a process which opened /var/log/syslog can be done by

```
# lsof -t /var/log/syslog
489
```
Talking about kill, did you know that there are 4 Ways to Kill a Process?

###  Combine more list options using OR/AND

By default when you use more than one list option in lsof, they will be ORed. For example,

```
# lsof -u lakshmanan -c init

COMMAND    PID       USER   FD   TYPE     DEVICE SIZE/OFF       NODE NAME
init         1       root  cwd    DIR        8,1     4096          2 /
init         1       root  txt    REG        8,1   124704     917562 /sbin/init
bash      1995 lakshmanan    2u   CHR      136,2      0t0          5 /dev/pts/2
bash      1995 lakshmanan  255u   CHR      136,2      0t0          5 /dev/pts/2
...
```

The above command uses two list options, ‘-u’ and ‘-c’. So the command will list process belongs to user ‘lakshmanan’ as well as process name starts with ‘init’.

But when you want to list a process belongs to user ‘lakshmanan’ and the process name starts with ‘init’, you can use ‘-a’ option.

```
# lsof -u lakshmanan -c init -a
```

The above command will not output anything, because there is no such process named ‘init’ belonging to user ‘lakshmanan’.

###  Execute lsof in repeat mode

lsof also support Repeat mode. It will first list files based on the given parameters, and delay for specified seconds and again list files based on the given parameters. It can be interrupted by a signal.

Repeat mode can be enabled by using ‘-r’ or ‘+r’. If ‘+r’ is used then, the repeat mode will end when no open files are found. ‘-r’ will continue to list,delay,list until a interrupt is given irrespective of files are opened or not.

Each cycle output will be separated by using ‘=======’. You also also specify the time delay as ‘-r’ | ‘+r’.

```
# lsof -u lakshmanan -c init -a -r5

=======
=======
COMMAND   PID       USER   FD   TYPE DEVICE SIZE/OFF    NODE NAME
inita.sh 2971 lakshmanan  cwd    DIR    8,1     4096  393218 /home/lakshmanan
inita.sh 2971 lakshmanan  rtd    DIR    8,1     4096       2 /
inita.sh 2971 lakshmanan  txt    REG    8,1    83848  524315 /bin/dash
inita.sh 2971 lakshmanan  mem    REG    8,1  1434180 1442625 /lib/i386-linux-gnu/libc-2.13.so
inita.sh 2971 lakshmanan  mem    REG    8,1   117960 1442612 /lib/i386-linux-gnu/ld-2.13.so
inita.sh 2971 lakshmanan    0u   CHR  136,4      0t0       7 /dev/pts/4
inita.sh 2971 lakshmanan    1u   CHR  136,4      0t0       7 /dev/pts/4
inita.sh 2971 lakshmanan    2u   CHR  136,4      0t0       7 /dev/pts/4
inita.sh 2971 lakshmanan   10r   REG    8,1       20  393578 /home/lakshmanan/inita.sh
=======
```

In the above output, for the first 5 seconds, there is no output. After that a script named “inita.sh” is started, and it list the output.

Finding Network Connection

Network connections are also files. So we can find information about them by using lsof.

###  List all network connections

You can list all the network connections opened by using ‘-i’ option.

```
# lsof -i

COMMAND    PID  USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
avahi-dae  515 avahi   13u  IPv4   6848      0t0  UDP *:mdns
avahi-dae  515 avahi   16u  IPv6   6851      0t0  UDP *:52060
cupsd     1075  root    5u  IPv6  22512      0t0  TCP ip6-localhost:ipp (LISTEN)
```

You can also use ‘-i4′ or ‘-i6′ to list only ‘IPV4′ or ‘IPV6‘ respectively.


###  List all network files in use by a specific process

You can list all the network files which is being used by a process as follows

```
# lsof -i -a -p 234
```

You can also use the following

```
# lsof -i -a -c ssh
```

The above command will list the network files opened by the processes starting with ssh.

###  List processes which are listening on a particular port

You can list the processes which are listening on a particular port by using ‘-i’ with ‘:’ as follows

```
# lsof -i :25

COMMAND  PID        USER   FD   TYPE DEVICE SIZE NODE NAME
exim4   2541 Debian-exim    3u  IPv4   8677       TCP localhost:smtp (LISTEN)
```

###  List all TCP or UDP connections

You can list all the TCP or UDP connections by specifying the protocol using ‘-i’.

```
# lsof -i tcp; lsof -i udp;
```

###  List all Network File System ( NFS ) files

You can list all the NFS files by using ‘-N’ option. The following lsof command will list all NFS files used by user ‘lakshmanan’.

```
# lsof -N -u lakshmanan -a
```