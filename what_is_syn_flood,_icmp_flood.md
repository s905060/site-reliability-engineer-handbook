#What is SYN Flood, ICMP Flood

SYN Flood : A SYN flood occurs when a host sends a flood of TCP/SYN packets, often with a fake/forged sender address. Each of these packets is handled like a connection request, causing the server to spawn a half-open connection, by sending back a TCP/SYN-ACK packet(Acknowledge), and waiting for a packet in response from the sender address(response to the ACK Packet). However, because the sender address is forged, the response never comes. These half-open connections saturate the number of available connections the server is able to make, keeping it from responding tolegitimate requests until after the attack ends


ICMP Flood : There are three types of ICMP Flood :
1. [Smurf Attack ](http://en.wikipedia.org/wiki/Smurf_attack)
2. [Ping Flood](http://en.wikipedia.org/wiki/Ping_flood)
3. [Ping of Death](http://en.wikipedia.org/wiki/Ping_of_death)
