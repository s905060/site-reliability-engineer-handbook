# Traceroute

Traceroute makes use of this TTL exceeded messages to find out routers that come across your path to destination(Because these exceeded messages send by the router will contain its address).

* Step 1: My Source address will make a packet with destination ip address of 8.8.8.8 and a destination port number between 33434 to 33534. And the important thing it does it to make the TTL Value 1
 
* Step 2: Of course my packet will reach my gateway server. On seeing receiving the packet my gateway server will reduce the TTL by 1 (All routers/hops in between does this job of reducing the TTL value by 1). Once the TTL is reduced by the value of 1 (1-1= 0), the TTL value becomes zero. Hence my gateway server will send me back a TTL Time exceeded message. Please remember that when my gateway server sends a TTL exceeded message back to me, it will send the first 28 byte header of the initial packet i send.
 
* Step 3:  On receiving this TTL Time exceeded message, my traceroute program will come to know the source address and other details about the first hop (Which is my gateway server.).
 
* Step 4: Now the traceroute program will again send the same UDP packet with the destination of 8.8.8.8, and a random UDP destination port between 33434 to 33534. But this time i will make the initial TTL 2.  This is because my gateway router will reduce it by 1 and then forwards that same packet which send to the next hop/router (the packet send by my gateway to its next hop will have a TTL value of 1).
 
* Step 5: On receiving UDP packet, the next hop to my gateway server will once again reduce it to 1 which means now the TTL has once again become 0. Hence it will send me back a ICMP Time exceeded message with its source address, and also the first 28 byte header of the packet which i send.
 
* Step 6: On receiving that message of TTL Time Exceeded, my traceroute program will come to know about that hop/routers IP address and it will show that on my screen.
 
* Step 7: Now again my traceroute program will make a similar UDP packet with again a random udp port with the destination address of 8.8.8.8. But this time the ttl value is made to 3, so that the ttl will automatically become 0, when it reaches the third hop/router(Please remember that my gateway and the next hop to it, will reduce it by 1 ). So that it will reply me with a TTL Time exceeded message, and my traceroute program will come to know about that hop/routers IP  address.

* Step 8: 
On receiving that reply, the traceroute program will once again make a UDP packet with TTL value of 4 this time. If i gets a TTL Time exceeded for that also, then my traceroute program will send a UDP packet with TTL of 5 and so on.