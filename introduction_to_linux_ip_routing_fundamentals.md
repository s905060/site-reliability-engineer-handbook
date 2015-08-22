# Introduction to Linux IP Routing Fundamentals 

Probably you know how to check the existing routes (or even add/modify routes) on Linux using route or netstat command. You migh’ve done that without understanding much about how IP routing works.

This article will help you understand the principles behind the IP routing and how it works.

This is the 1st part in the IP Routing series of articles.

IP routing involves forwarding of IP datagrams. Its a simple process in which the host sends the IP datagram directly to the destination if the destination host is connected. For example, through a point to point link or through a shared network. If the destination host is not directly connected then the host sends the IP datagram to the default router and lets the router decide where to send the IP datagram further.

###Routing Principles

A fundamental difference between a normal host and a router is that a host never forwards a datagram from one interface to other while a router can forward a datagram.

Today, most of the multiuser systems can be configured to act as a router. So, a common routing algorithm can be specified that can be used by the router as well as by a host. When a host can act like a router, it is generally said that the host has an embedded router functionality. Such a host which has an embedded router functionality should never forward datagrams until and unless configured to do so ie configured to act like a router.

IP layer maintains a routing table which it uses while making decisions about what to do with the datagram received. When the data gram is received from the network then IP layer first checks its IP address to see if the IP address is of its own or not.

In case the destination IP address in the datagram is of its own then the datagram is sent to the appropriate protocol at the transport layer but if the destination address is not of its own then the datagram is forwarded if the host was configured to act like a router otherwise the datagram is discarded.

The data in routing table is generally in the form of entries. A typical routing table entry contains the following main entries :

**Destination IP address** : This  field represents the IP address of the destination. This IP address could be the address of a single host or could that be of a network. If this entry contains the IP address of a host then it is signified by a non zero host ID in the address while if the entry contains the IP address of a network then it is signified by a host ID value of 0.

**IP address of next router** : Why have we used the term ‘next’ is because this is not always the final router but it could very well be an intermediate router. This entry gives the IP address of the next router which decides how to further send an IP data gram received on its interface.

**Flags** : This field provides another set of vital information like destination IP address (specified above) is a host address or a network address. Also, flags convey whether the next router (specified above) is really a next router or a directly connected interface.

**Network interface specs** : Some specification about the network interface the datagram should be passed for further transmission.

### How Basic Routing Works?

So if we briefly try to visualize the routing process now, then we see that as soon as a datagram from a network is received at the IP layer of a host (which is configured to act like a router) then after verifying that the destination IP address in the packet is not that of this host the routing tables are consulted.

Any entry whose first field matches the destination IP address completely(a host) or partially (a network) would signal the IP address of the next router. This is the vital information that a host (acting like a router) would require to forward a packet as this information directly tells on which next router the datagram should be forwarded to. All the other fields in the entry support the decision making by providing more information for routing.

In the paragraph above we build a basic understanding but if we try to get a level deeper then the following points give the detailed information about the routing table algorithm:

* First the routing table is searched for an entry whose ‘Destination IP address’ field matches the datagram destination IP address completely.  By completely, it is meant that the host ID and network ID of the IP addresses match. If found, then the datagram is sent to that interface or to the intermediate router.

* If a complete match is not found then a search for matching network ID is done. If found then the datagram is forwarded to the indicated router. So we see that all the hosts on this network are managed by this single entry in the routing table.

* If none of the above two is true then the datagram is forwarded to a ‘default router’.

* If the above step also fails ie there is not default router then the datagram ends up being undeliverable. Any undeliverable datagram would produce an ICMP host unreachable or ICMP network unreachable error and this error is returned to the application that generated this datagram.

Sometimes one would ask as to why there are two types of entries in the routing table or to be more precise why network related entries are needed in a router? Well, having entries in routing table corresponding to networks has a big advantage. The advantage is that by having an entry related a complete network avoids the need to have a huge number of separate entries of each host on that network. This brings down the size of the routing table to a significant level which is always good.

### Command to list routing tables

You can use netstat command to list the routing tables as shown below.
```
$ netstat -rn
Kernel IP routing table
Destination  Gateway         Genmask         Flags   MSS Window  irtt Iface
192.168.2.0  0.0.0.0         255.255.255.0   U         0 0        0    eth0
169.254.0.0  0.0.0.0         255.255.0.0     U         0 0        0    eth0
0.0.0.0      192.168.2.1     0.0.0.0         UG        0 0        0    eth0
```

The output provides a detailed information in the destination IP addresses and their gateways. The flag ‘U’ suggests that the route is up and the flag ‘G’ suggests that the router is to a gateway (router). If this flag is not set then it can be assumed that the destination is directly connected.