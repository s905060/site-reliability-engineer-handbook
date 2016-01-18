# The architecture of clustering Graphite

###Clarifying the Components

Graphite isn't directly comparable to say, Zenoss. It's not a monolithic, boxed-up, poll-based SNMP system. It scales and bottlenecks a bit differently than what you might be used to. It's the fundamental operations that need to be understood in order to break out the components properly, and I think this is where the 'installation guides' I've mentioned skimp out.

So while you can find plenty of those (in case you are looking for basic installation), I'll go over what the components are and how they interact.