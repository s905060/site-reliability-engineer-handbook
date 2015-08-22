# Tcpdump

tcpdump is a network packet analyzer. Using tcpdump you can capture the packets and analyze it for any performance bottlenecks.

The following tcpdump command example displays captured packets in ASCII.

```
$ tcpdump -A -i eth0
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on eth0, link-type EN10MB (Ethernet), capture size 96 bytes
14:34:50.913995 IP valh4.lell.net.ssh > yy.domain.innetbcp.net.11006: P 1457239478:1457239594(116) ack 1561461262 win 63652
E.....@.@..]..i...9...*.V...]...P....h....E...>{..U=...g.
......G..7\+KA....A...L.
14:34:51.423640 IP valh4.lell.net.ssh > yy.domain.innetbcp.net.11006: P 116:232(116) ack 1 win 63652
E.....@.@..\..i...9...*.V..*]...P....h....7......X..!....Im.S.g.u:*..O&....^#Ba...
E..(R.@.|.....9...i.*...]...V..*P..OWp........
```

Using tcpdump you can capture packets based on several custom conditions. For example, capture packets that flow through a particular port, capture tcp communication between two specific hosts, capture packets that belongs to a specific protocol type, etc.