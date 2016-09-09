# ARP

This command is more advanced, yet still exceptionally useful when configuring networks. It observes and alters so called ARP table. ARP stands for Address Resolution Protocol. ARP table defines relationships between MAC address and IP address. In particular, for every IP address, it defines appropriate MAC address. This used when computer decides to send packet to certain IP address and it has to find MAC address for the IP address. This is when ARP table becomes useful. Computer checks if IP address is in the table and if so picks MAC address from it. If IP address is not in the table, computer uses ARP protocol to find it. arp command used to observe and manually alter ARP table entries.


### List ARP table

In its basic form, when invoked, it prints content of the ARP table.
```bash
$ arp
Address             HWtype  HWaddress           Flags Mask       Iface
192.168.2.174       ether   00:11:25:9B:7F:74   C                eth0
192.168.2.2         ether   00:17:65:C7:10:45   C                eth0
```

In this example, ARP table on my computer contains two entries. Note the HWtype column. It is common understanding that MAC address refers to Ethernet MAC address, but its not necessarily true. There are many L2 protocols and some have their own MAC address structure. For more information about L2 and protocols that belong to this layer see OSI model on Wikipedia.


### Add new ARP entry


There are two things that you would probably like to do with ARP table; add and remove entries. This is how you add a new entry.
```bash
arp -s <ip address> <hardware address>
```

Again, hardware address is mostly Ethernet MAC address, but it is not always necessarily true.


### Delete ARP entry


```bash
arp -d <ip address>
arp -d <hardware address>
```

Both forms of this command delete the specified ARP address. First uses hostname or IP address to identify the ARP entry that we would like to delete. Second uses hardware address to identify appropriate ARP entry.
