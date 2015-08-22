# Hubs vs Switches vs Routers – Networking Device Fundamentals

Most of the systems you are working on might be connected to a hub, or switch, or router. Probably you never thought about those networking devices, how they work, and the differences between them.

In this article, we’ll explain the core technical differences between these networking devices.

To understand these, it is also helpful if you have some basic knowledge of different layers in OSI model of communication.

### Hubs

* Hubs, also known as repeaters, are network devices that can operate on layer-1 (I.e. the physical layer) to connect network devices for communication.
* Hubs cannot process layer-2 or layer-3 traffic. Layer-2 deals with hardware addresses and layer-3 deals with logical (IP) addresses. So, hubs cannot process information based on MAC or IP addresses.
* Hubs cannot even process data based on whether it is a uni-cast, broadcast or multi-cast data.
All that a hub does is that it transfers data to every port excluding the port from where data was generated.
* Hubs work only in half duplex mode I.e. a device connected to a hub can either send or receive data at a given time.
* If more than one device sends out data simultaneously then data collisions happen.
* In case of a collision, a hub rejects data from all the devices and signals them to send data again. Usually devices follow a random timer after which data is sent again to hub.
* Hubs are prone to collisions and as more and more devices are added to set up of multiple hubs, the chances of collisions will increase and hence the overall performance of network will go down.

### Switches

* Switches are network devices that operate on layer-2 of OSI model of communication.
* Switches are also known as intelligent hubs.
* Switches operate on hardware addresses to transfer data across devices connected to them.
* The reason switches are known as intelligent hubs is because they build address table in hardware to keep track of different hardware addresses and the port to which each hardware address is associated.
* The reason why they are compared to hubs because a switch, when started fresh, acts just like a hub. Suppose there are 3 devices connected to a switch. Lets call these devices as deviceA, deviceB and deviceC. Now, after a fresh start, if deviceA sends out a message to deviceB then just like a hub, switch will send it out to each port. But, it will store the hardware address and corresponding port in its hardware table. This means that whenever any other device will send any packet destined to deviceA then switch will act intelligently and send it to the correct port and not to all the ports. This way as more and more interaction takes place, the hardware table of switch grows and after a certain period of time switch becomes full blown intelligent version of a hub.
* Switches are often confused with bridges. Though both of them are mostly similar with major difference being that a switch forwards data at wire speed as it uses special hardware circuits known as ASICs.
* Switches, unlike hubs, support full duplex data transfer communication for each connected device.
As layer 2 protocols headers have no information about network of data packet so switches cannot forward data based or networks and that is the reason switches cannot be used with large networks that are divided in sub networks.
* Switches can avoid loops through the use of spanning tree protocol.

### Routers

Routers are the network devices that operate at Layer-3 of OSI model of communication.
As layer-3 protocols have access to logical address (IP addresses) so routers have the capability to forward data across networks.
Sometimes routers are also known as layer-3 switches.
Routers are far more feature rich as compared to switches.
Routers maintain routing table for data forwarding.
Earlier, routing was slower as compared to switching. This was because of the fact that routing table lookup time was considerably high. The reason for this was that the complete packet was fetched into software buffers and then further operations were carried on it.
Today, operations are done in hardware which has reduced the latency a lot and hence routers are not considered slower than switches today.
Routers have lesser port densities as compared to switches.
Routers are usually used as a forwarding network elements in Wide Area Networks.
