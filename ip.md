# IP

Can you explain the ip command to setup routing on Linux based systems? How do I use the ip command to configure the routing table of the Linux kernel?

The ip command can be used for the following tasks on Linux:
=> Show / manipulate routing

=> Show / manipulate devices

=> Policy routing

=> Tunnels

Task: View / Display Routing Table

Type the following command:
```
$ ip route show
```
OR
```
$ ip route list
```
Sample Outputs:
```
10.0.31.18 dev ppp0  proto kernel  scope link  src 10.1.3.103
192.168.2.0/24 dev eth0  proto kernel  scope link  src 192.168.2.1
192.168.1.0/24 dev ra0  proto kernel  scope link  src 192.168.1.106
169.254.0.0/16 dev eth0  scope link  metric 1000
10.0.0.0/8 dev ppp0  scope link
default via 192.168.1.1 dev ra0  metric 100
```

Each entry is nothing but an entry in the routing table (Linux kernel routing table). For example. following line represents the route for the local network. All network packets to a system in the same network are sent directly through the device ra0:

192.168.1.0/24 dev ra0  proto kernel  scope link  src 192.168.1.106

Our default route is set via ra0 interface i.e. all network packets that cannot be sent according to the previous entries of the routing table are sent through the gateway defined in this entry i.e 192.168.1.1 is our default gateway.

Task: Set a Route to the Locally Connected Network eth0

Type the following command to sent all packets to the local network 192.168.1.0 directly through the device eth0:, enter:
```
# ip route add 192.168.1.0/24 dev eth0
```

Task: Set a default route

All network packets that cannot be sent according to the previous entries of the routing table are sent through the following default gateway
```
# ip route add default via 192.168.1.254
```

Task: Delete route from table

Type the following command
```
# ip route delete 192.168.1.0/24 dev eth0
```
How do I verify routing configurations?

Use the ping/host commands to make sure you can reach to your gateway:
```
ping Your-Gateway-Ip-Here
ping Your-DNS-Server-IP-Here
ping 192.168.1.254
ping www.cyberciti.biz
host www.cyberciti.biz
```
