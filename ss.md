# ss - socket statistics

In a previous tutorial we saw how to use the netstat command to get statistics on network/socket connections. However the netstat command has long been deprecated and replaced by the ss command from the iproute suite of tools.

The ss command is capable of showing more information than the netstat and is faster. The netstat command reads various /proc files to gather information. However this approach falls weak when there are lots of connections to display. This makes it slower.

The ss command gets its information directly from kernel space. The options used with the ss commands are very similar to netstat making it an easy replacement.

So in this tutorial we are going to see few examples of how to use the ss command to check the network connections and socket statistics.

### 1. List all connections
The simplest command is to list out all connections.

```
$ ss | less
Netid  State      Recv-Q Send-Q   Local Address:Port       Peer Address:Port   
u_str  ESTAB      0      0                    * 15545                 * 15544  
u_str  ESTAB      0      0                    * 12240                 * 12241  
u_str  ESTAB      0      0      @/tmp/dbus-2hQdRvvg49 12726                 * 12159  
u_str  ESTAB      0      0                    * 11808                 * 11256  
u_str  ESTAB      0      0                    * 15204                 * 15205  
.....
```
