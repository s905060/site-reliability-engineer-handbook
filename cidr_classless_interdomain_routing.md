# CIDR (Classless InterDomain Routing)

CIDR (Classless Inter-Domain Routing) was introduced in 1993 (RCF 1517) replacing the previous generation of IP address syntax - classful networks. CIDR allowed for more efficient use of IPv4 address space and prefix aggregation, known as route summarization or supernetting.

CIDR introduction allowed for:

* More efficient use of IPv4 address space
* Prefix aggregation, which reduced the size of routing tables

CIDR allows routers to group routes together to reduce the bulk of routing information carried by the core routers. With CIDR, several IP networks appear to networks outside the group as a single, larger entity. With CIDR, IP addresses and their subnet masks are written as four octets, separated by periods, followed by a forward slash and a two-digit number that represents the subnet mask e.g.

**10.1.1.0/30**

**172.16.1.16/28**

**192.168.1.32/27** etc.

 

CIDR / VLSM Network addressing topology example