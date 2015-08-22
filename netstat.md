# Netstat

What is command to check ports runnin­g/used over local machine

```
$netstat -antp
```

Netstat command displays various network related information such as network connections, routing tables, interface statistics, masquerade connections, multicast memberships etc.,

The following are some netstat command examples.

List all ports (both listening and non listening) using netstat -a as shown below.

```
# netstat -a | more
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address           Foreign Address         State
tcp        0      0 localhost:30037         *:*                     LISTEN
udp        0      0 *:bootpc                *:*                                

Active UNIX domain sockets (servers and established)
Proto RefCnt Flags       Type       State         I-Node   Path
unix  2      [ ACC ]     STREAM     LISTENING     6135     /tmp/.X11-unix/X0
unix  2      [ ACC ]     STREAM     LISTENING     5140     /var/run/acpid.socket
```

Use the following netstat command to find out on which port a program is running.

```
# netstat -ap | grep ssh
(Not all processes could be identified, non-owned process info
 will not be shown, you would have to be root to see it all.)
tcp        1      0 dev-db:ssh           101.174.100.22:39213        CLOSE_WAIT  -
tcp        1      0 dev-db:ssh           101.174.100.22:57643        CLOSE_WAIT  -
```

Use the following netstat command to find out which process is using a particular port.

```
# netstat -an | grep ':80'
```