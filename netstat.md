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

How to list all port using netstat ?
```
# netstat -a
```
List all listening ports?
```
# netstat -l
```
List all tcp ports ?
```
# netstat -at

Example:
# netstat -at
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address               Foreign Address             State
tcp        0      0 *:mysql                     *:*                         LISTEN
tcp        0      0 *:submission                *:*                         LISTEN
```

List all udp port ?
```
# netstat -au
Example:

# netstat -au
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address               Foreign Address             State
udp        0      0 host.org:domain  *:*
udp        0      0 localhost.localdomain:domain *:*
```
List all Unix ports ?
```
# netstat -ax
```
List all listening tcp ports?
```
# netstat -lt
```
List all listening udp ports?
```
# netstat -lu
```
List all listening Unix ports ?
```
# netstat -lx
```
List all tcp ports with Process ID and numerically ?
You can use the -n and -p to do the same.
```
# netstat -atp
```
List all tcp ports which are in listening status with Process ID and numerically ?
You can use the -n and -p to do the same.
```
# netstat -ltp
```