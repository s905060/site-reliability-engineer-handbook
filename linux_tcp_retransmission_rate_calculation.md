# Linux TCP retransmission rate calculation

TCP retransmission rate is a manifestation of the network quality, simple packaging netstat -s output can be calculated TCP retransmission rate. Ready-made script as follows:



Possible causes high rate of TCP retransmissions
Description Network transmission occurs retransmission packet loss, basically from three points to locate: the client network, the service side of the network, the intermediate link network conditions

* Client machine network anomalies
* Traffic run over the server NIC, NIC packet loss, attention ifconfig the error output
* Intermediate network connection road congestion, such as the switch-linked core switch links, etc., required to check each link traffic situation