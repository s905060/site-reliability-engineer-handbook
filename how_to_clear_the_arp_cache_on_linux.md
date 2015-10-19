# How to clear the ARP cache on Linux?

In some cases you might need to clear your ARP cache. There are two common ways on Linux, using the arp or ip utility.

Clearing cache with arp
The arp utility does not accept an option to clear the full cache. Instead, it allows to flush out entries found with the -d option.
```
root@ubuntu:~# arp -d 192.168.1.1
```
After deleting, have a look with the arp utility again to see the new list:
```
root@ubuntu:~# arp -n
Address                  HWtype  HWaddress           Flags Mask            Iface
192.168.1.1                      (incomplete)                              eth0
192.168.1.2              ether   00:02:9b:a2:d3:f3   C                     eth0
192.168.1.3              ether   00:02:9b:d9:d1:a2   C                     eth0
```
Clearing cache with ip
Newer Linux distributions have the ip utility, which has a more advanced way to clear out the full ARP cache
```
root@ubuntu:~# ip -s -s neigh flush all
192.168.1.1 dev eth0 lladdr 00:a1:04:c6:10:14 used 757/757/28 probes 6 STALE
192.168.1.2 dev eth0 lladdr 00:02:9b:a2:d3:f3 used 2555/719/659 probes 6 STALE
192.168.1.3 dev eth0 lladdr 00:02:9b:d9:d1:a2 ref 1 used 0/0/0 probes 6 DELAY
```
*** Round 1, deleting 3 entries ***
*** Flush is complete after 1 round ***
The first -s will provide a more verbose output. The second one defines the neighbor table, which equals the ARP and NDISC cache.

Conclusion
Depending on your distribution, the ip utility is quicker if you want to flush out the full ARP cache. For individual entries the arp tool will do the job as quickly.