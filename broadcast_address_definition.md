# Broadcast Address Definition

A broadcast address is a special type of networking address that is reserved for sending messages to all nodes (i.e., devices attached to the network) on a given network or network segment.

A network segment is a portion of a computer network that is separated from the rest of the network by a device such as a repeater, hub, bridge, switch or router. Each segment can contain one or multiple computers or other end devices (e.g., printers, modems or disk drives). A broadcast is the simultaneous transmission of a single message to all nodes on the network or on a network segment.

A broadcast address is usually a MAC address consisting of all F's (i.e., 0xFFFFFFFF). A MAC (media access control) address is a unique 48-bit hexadecimal (i.e., base 16) code that is permanently assigned to each network interface card (NIC) and other unit of network hardware by the manufacturer at the factory.

On IP (Internet protocol) networks, the general broadcast address is 255.255.255.255, which is equivalent to the 0xFFFFFFFF MAC address (i.e., both are all ones in binary code).

The broadcast address for a specific network includes all ones in the host portion of the IP address. On class C networks, for example, the host portion is the last byte (i.e., the fourth segment), and on class B networks it is the final two bytes. Thus, for instance, the broadcast address for the class C network 192.168.3.0 would be 192.168.3.255.