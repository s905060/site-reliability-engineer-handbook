# EtherChannel Switch Link Aggregation

### What is EtherChannel

As a rule of thumb, network resilience should be your number one priority. In order to achieve High Availability and Uninterrupted Service Continuity you should make sure that you have duplicate hardware devices as well as duplicate interconnection links, where appropriate. We have learned that having multiple connection links between network switches imposes packet loops unless a protection mechanism such as STP is implemented. But we also know that STP disables all redundant links. Therefore, with STP running we could never be able to implement backup interconnection links.

Do not get disappointed, for there is a solution to this problem. The solution is called “EtherChannel”. The idea is to have multiple physical links between switches combined into a single logical link with built-in mechanisms to prevent packet loops. The benefits of aggregating switch links are:

* Increased perceived redundancy level; a single or double link failure between two switches can be immediately overcome.
 
* Expansion of bandwidth between switches; added bandwidth requirements can be satisfied by bundling interconnection links.