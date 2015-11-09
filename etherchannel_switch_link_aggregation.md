# EtherChannel Switch Link Aggregation

### What is EtherChannel

As a rule of thumb, network resilience should be your number one priority. In order to achieve High Availability and Uninterrupted Service Continuity you should make sure that you have duplicate hardware devices as well as duplicate interconnection links, where appropriate. We have learned that having multiple connection links between network switches imposes packet loops unless a protection mechanism such as STP is implemented. But we also know that STP disables all redundant links. Therefore, with STP running we could never be able to implement backup interconnection links.

Do not get disappointed, for there is a solution to this problem. The solution is called “EtherChannel”. The idea is to have multiple physical links between switches combined into a single logical link with built-in mechanisms to prevent packet loops. The benefits of aggregating switch links are:

* Increased perceived redundancy level; a single or double link failure between two switches can be immediately overcome.
 
* Expansion of bandwidth between switches; added bandwidth requirements can be satisfied by bundling interconnection links.

Prerequisites of Enabling EtherChannel
Nothing comes without a price. Fortunately the prerequisites for enabling EtherChannel between two switches are simple and easy to accomplish. These are:

* EtherChannel bundle links must be of the same type and speed. For example if we intend to combine FastEthernet links (100Mbps) then all bundle links MUST be FastEthernet links of 100Mbps speed.

* All links intended to be bundled MUST NOT exceed the number of eight physical links. Up to eight physical links can be combined into one logical link.

* All links MUST belong to the same VLAN if used as access links. They MUST carry the same VLANS if used as trunk links and MUST be configured with identical STP settings.

### EtherChannel Negotiation Protocols

Link aggregation wouldn’t exist if it wasn’t for Control protocols to negotiate and establish EtherChannel connectivity. On Catalyst switches, two EtherChannel negotiation protocols are supported:

* Port Aggregation Protocol (PAgP): Cisco Proprietary protocol; EtherChannel negotiation is carried out on EtherChannel capable ports in order to achieve bundling of physical links.

* Link Aggregation Control Protocol (LACP): Standardized protocol defined in IEEE 802.3ad; Similar to PAgP. Nevertheless, LACP, has the following major differences compared to PAgP:

– LACP assigns roles to interconnected switches, so that the master switch is allowed to make decisions on which ports are participating in the EtherChannel at any given time.

– Up to sixteen switch links can be part of the bundle group, though, eight of them can be active at the same time. Activation of a standby links is performed only when an active link goes down.

### EtherChannel Load Balancing Methods

Various methods exist for efficiently distributing traffic load across bundled links. There is no “best method.” The effectiveness of each method relies upon the individual network architecture and the specific traffic architecture. The most important EtherChannel load distribution methods are:

