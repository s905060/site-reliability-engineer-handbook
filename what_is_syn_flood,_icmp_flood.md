#What is SYN Flood, ICMP Flood

SYN Flood : A SYN flood occurs when a host sends a flood of TCP/SYN packets, often with a fake/f­orged sender address. Each of these packets is handled like a connection request, causing the server to spawn a half-open connec­tion, by sending back a TCP/SY­N-ACK packet­(Ac­kno­wle­dge), and waiting for a packet in response from the sender addres­s(r­esponse to the ACK Packet). However, because the sender address is forged, the response never comes. These half-open connec­tions saturate the number of available connec­tions the server is able to make, keeping it from responding tolegi­timate requests until after the attack ends


ICMP Flood : There are three types of ICMP Flood :
1. [Smurf Attack ](http:/­/en.wi­kip­edi­a.o­rg/­wik­i/S­mur­f_a­ttack)
2. [Ping Flood](http:/­/en.wi­kip­edi­a.o­rg/­wik­i/P­ing­_flood)
3. [Ping of Death](http:/­/en.wi­kip­edi­a.o­rg/­wik­i/P­ing­_of­_death)
