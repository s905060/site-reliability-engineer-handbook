# Ifconfig

Ifconfig command is used to configure network interfaces. ifconfig stands for interface configurator. Ifconfig is widely used to initialize the network interface and to enable or disable the interfaces.

In this article, let us review 7 common usages of ifconfig command.


###1. View Network Settings of an Ethernet Adapter

Ifconfig, when invoked with no arguments will display all the details of currently active interfaces. If you give the interface name as an argument, the details of that specific interface will be displayed.

```
# ifconfig eth0

eth0   Link encap:Ethernet  HWaddr 00:2D:32:3E:39:3B
inet addr:192.168.2.2  Bcast:192.168.2.255  Mask:255.255.255.0
inet6 addr: fe80::21d:92ff:fede:499b/64 Scope:Link
UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
RX packets:977839669 errors:0 dropped:1990 overruns:0 frame:0
TX packets:1116825094 errors:8 dropped:0 overruns:0 carrier:0
collisions:0 txqueuelen:1000
RX bytes:2694625909 (2.5 GiB)  TX bytes:4106931617 (3.8 GiB)
Interrupt:185 Base address:0xdc00
```

###2. Display Details of All interfaces Including Disabled Interfaces

```
# ifconfig -a
```

###3. Disable an Interface

```
# ifconfig eth0 down
```

###4. Enable an Interface

```
# ifconfig eth0 up
```

###5. Assign ip-address to an Interface

Assign 192.168.2.2 as the IP address for the interface eth0.

```
# ifconfig eth0 192.168.2.2
```

Change Subnet mask of the interface eth0.

```
# ifconfig eth0 netmask 255.255.255.0
```

Change Broadcast address of the interface eth0.

```
# ifconfig eth0 broadcast 192.168.2.255
```

Assign ip-address, netmask and broadcast at the same time to interface eht0.

```
# ifconfig eth0 192.168.2.2 netmask 255.255.255.0 broadcast 192.168.2.255
```

###6. Change MTU

This will change the Maximum transmission unit (MTU) to XX. MTU is the maximum number of octets the interface is able to handle in one transaction. For Ethernet the Maximum transmission unit by default is 1500.

```
# ifconfig eth0 mtu XX
```

###7. Promiscuous mode

By default when a network card receives a packet, it checks whether the packet belongs to itself. If not, the interface card normally drops the packet. But in promiscuous mode, the card doesn’t drop the packet. Instead, it will accept all the packets which flows through the network card.

Superuser privilege is required to set an interface in promiscuous mode. Most network monitor tools use the promiscuous mode to capture the packets and to analyze the network traffic.


Following will put the interface in promiscuous mode.

```
# ifconfig eth0 promisc
```

Following will put the interface in normal mode.

```
# ifconfig eth0 -promisc
```
