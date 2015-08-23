# Journey of a Data Packet in the Internet

While majority of the end-users doesn’t care how Internet works, some of you might be curious to understand the basics of how Internet works.

In this article we will try to peel off the first layer on this topic to understand how Internet works by elaborating the journey of a data packet from its source to destination on the Internet. From this perspective, we’ll try to keep the content of this article fairly basic.

Before going further, lets first quickly and briefly understand the concepts of DHCP and DNS.

### DHCP

Have you ever thought how your computer gets an IP address? Well, it is important to know that there are two ways through which a computer gets an IP address. One is static while the other is dynamic.

Static method is the one in which the computer administrator manually sets the IP address to the machine. If your machine is connected to a network like LAN then one thing is to be kept in mind that the IP address being set should not be the same as the IP address of any other machine on the same network as this may lead to IP address conflict and none of the two machines will be able to access the internet.

Dynamic method is the one in which the computer (on system boot) asks a server to assign an IP address to it. The protocol used for this process is known as Dynamic Host Control Protocol (DHCP). The server referenced here is known DHCP server. This server is responsible for assigning IP addresses to all the computers on the network. It is the responsibility of the DHCP server to make sure that there is no IP address conflict. If one of the machine goes down and then again boots up then a fresh DHCP request is sent to the server which may assign the same or some different IP address this time.  Usually a pool of IP addresses is given to the DHCP server and it uses only those IP addresses for assignments. This is done to safely use other IP addresses for static assignments without any conflict.

### DNS

Most of us would have used google.com for internet search but have you ever thought on how it is made sure that typing google.com in our web browser will actually contact the correct server? Well, to understand this, we need to understand the concept of Domain name server (DNS).

In real life as people are identified by their name, similarly in computer networks, individual computers are identified through the IP address assigned to them. IP addresses can be of two types : public and private. Usually the servers use public IPs as they are contacted by millions of computers world wide. While your computer which is connected behind the router is usually assigned private IP. Since there is a limited number of public IPs that are available so the concept of private IPs in a network (behind a router with public IP) has grown popular and successful. The broader level concept used for this is known as NAT or Network address translation.

Remembering IP address is a bit difficult task for humans so each server also has a name (like google.com). So, end users just need to remember the name and type it in their web browser and hit enter. Now, the lets come to the story about what happens when the user hits enter after typing name in web browser. The first thing which is required is to convert the domain name to the corresponding IP. To accomplish this, a request is sent to the default gateway (which in most of the cases is the router) to contact the DNS server. The router has a configured DNS server IP to which this request is sent.

DNS servers are used to convert the domain name to IP address. When a request is received by the DNS server, it checks whether it has the required information. If this conversion information is not present then the DNS server forwards this request to the other DNS server. In this way, the domain name to IP address conversion is done and is sent back.

Once the IP is known then a normal HTTP GET request to that particular IP is made and things move on.

### Post DNS, how things move on?

To understand the following explanation one should have a basic knowledge of TCP/IP protocol suite layers. Still we’ll try to keep the explanation basic here.

* Once the IP address is known through the DNS process, an HTTP GET request is prepared at the application layer. This request is then forwarded to the Transport layer.

* There are two protocols (TCP and UDP) that are majorly used at this layer. It is at this layer the requests are encapsulated in form of transport layer packets. If TCP is being used then it also takes care that packet size should not exceed lowest MTU in the path between source and destination. This is done to avoid fragmentation of packet somewhere in the middle of its journey. On the other hand if UDP is being used then this special care is not taken and as a result packets can get fragmented.

* Once the packet is formed at transport layer, it is pushed to the IP layer. This layer adds the information related to source and destination IP addresses and some other important information like TTL (time to live), fragmentation information etc. All this information is required while the packet is on its way to the destination.

* After this the packet enters the data link layer where the information related to MAC addresses is added and then the packet is pushed on to the physical layer. So a stream of 0’s and 1’s is sent out of your NIC onto the physical media.
If the destination of the packet is not directly connected to the source computer then through the routing information present on the source computer, the packet is transmitted to the nearest relevant computer node. There can be various nodes in a network like routers, bridges, gateways etc. Each entity has its own importance like a router is used for forwarding the packet, a bridge is used for  connecting networks using same protocol while gateways are used for connecting networks with different protocols.

If we consider a basic network then routers are the main agents which play a vital role in forwarding the packet from source to destination. When the packet first leaves the source computer then the mac address of the relevant router (to which the packet is being transferred) is used as its destination mac address.

When the packet reaches to that router, then the router performs the following action :

* It decreases the TTL value and recomputes the check-sum of the packet.

* The router searches its routing information table for the complete host address as specified by the packet’s destination IP address. If found then router takes action to forward the packet to the relevant host.

* If no such entry is found then the table is searched for the network address derived from the destination IP. If found then router forwards the packet to that particular network.

* If above two checks fail then the packet is transferred to the the default router as derived from the default entry in its routing information table.

In any of the above cases, whenever the packet is transferred by router to some other router or to the destination, the destination mac address of the packet is changed to the immediate router or destination to which it is being sent. 

In this way the IP address information in the packet remains the same but the destination mac address changes from one router to another.  So in this way, the packet travels from one router to another until it reaches the destination.

Now, at the destination:

* The packet is first received at the physical layer which issues an IRQ to the CPU to indicate that some data is arrived and is waiting to be processed.

* After this the data is sent up to the data link layer where MAC layer is checked to see if this packet is indeed for this computer only.

* If the above check is passed then this packet is passed to IP layer where some IP address checks and check-sum verifications are done and then it is passed on to the relevant transport layer protocol.

* Once this is done, then from the knowledge of the ports the information (or the HTTP GET request in our case) is passed on the application listening on that port.

* This way the request reaches the google web server.
After this the response is formed and transmitted back in the same way as described above.

There you have it. This is how a data packet travels from source to destination in the Internet.

