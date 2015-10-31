# Juniper Most Used Junos Commands

###Version and Version Detail
show version: Lists which version of Junos OS is running on your device. It also shows the hostname of the device and the Juniper model number.

show version detail: Shows the version of all Junos processes running on the device.

###Chassis Hardware and Chassis Hardware Detail
show chassis hardware: Displays hardware inventory of the device and components installed in the device. Shows version, Juniper part number, serial number, and description of each component.

show chassis hardware detail: Displays version, part number, and serial number for all memory installed on device components.

###Configuration
configure: Accesses configuration mode.

show configuration: Displays the configuration currently running (active) on the device.

commit confirmed: Activates configuration changes, but returns to previous configuration automatically if you don’t actively accept the new configuration. Use: When you’re committing a configuration that you think may lock you out of the device or otherwise disrupt access to the device, use this command to guarantee that you’ll be able to log in to the device.

###Back Up and Roll Back
request system snapshot: Backs up the device’s file systems, including configurations.

rollback: Returns to the previously active device configuration.

file list detail /config and file list detail /var/db/config: Lists the backup configuration files on the device.

###Interfaces
show interfaces terse: Lists all interfaces (network cards) present in the box and shows whether they’re operational (up or down) and lists IP addresses of each interface. This command shows one interface per line, so it’s easily scannable.

show interfaces: Multiline output per interface lists properties of the physical (hardware) interface, including MAC address and hardware MTU, and of the logical (unit or subinterface) interface, including protocol MTU configured protocol addresses.

show interfaces interface-name: Multiline output for a single physical interface. Shows both physical and logical interface information.

show interfaces detail, show interfaces detail interface-name, show interfaces extensive, and show interfaces extensive interface-name: Show increasingly more detailed information about all interfaces or about a specific interface. The detail version adds interface statistics, and the extensive version adds error counters. Output is long, so you generally specify an interface name.

###Routing
show route: Lists the entries in all the device’s routing tables. Variants include the following:

* show route inet.0: Lists all IPv4 routes.

* show route inet.6l: Lists all IPv6 routes.

* show route detail: Adds route preference, next hop, and other information.

* show route protocol: Lists all routes learned by the specified routing protocol.

* show route forwarding-table: Lists the entries in all the device’s forwarding tables. Use: Lets you check which active routes are actually being used to forward traffic from the device toward network destinations.

show rip neighbor: Lists the RIP routers (neighbors) in the network.

show isis interface: Lists the device’s interfaces running IS-IS.

show isis adjacency: Lists the IS-IS routers (adjacencies) in the network.

show ospf interface: Lists the device interfaces running OSPF.

show ospf neighbor: Lists the IS-IS routers (neighbors) in the network.

show bgp neighbor: Lists the BGP routers to which this device is connected.

show bgp summary: Lists BGP group, peer, and session state information.

show route protocol bgp: Lists the routes learned from BGP.

###Switching
show Ethernet-switching interfaces: Lists information about the switched Ethernet interfaces.

show vlans: Lists the configured VLANs.

show virtual-chassis status: Lists the role and member ID assignments in a virtual-chassis configuration.

show spanning-tree bridge: Lists configured or calculated Spanning Tree Protocol parameters.

show spanning-tree interface: Lists configured or calculated interface-level Spanning Tree Protocol (STP) parameters.

###Maintenance
show log messages: Lists the system log messages in the default syslog file messages. The syslog family monitors all system-wide operations on the device and records them to syslog files. This command displays time-stamped entries so that you can see what has occurred on the device and when it occurred. Useful for tracking down device, network, and traffic flow problems.

show system uptime: Lists how long a device has been up and running. Shows you the last time that the device was powered on, restarted, or rebooted.