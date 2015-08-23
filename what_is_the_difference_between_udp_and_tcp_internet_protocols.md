# What is the difference between UDP and TCP internet protocols?

Transmission Control Protocol (TCP) and User Datagram Protocol (UDP)is a transportation protocol that is one of the core protocols of the Internet protocol suite. Both TCP and UDP work at transport layer TCP/IP model and both have very different usage.

### Difference between TCP and UDP

|TCP	| UDP
| --    | --
|Reliability: TCP is connection-oriented protocol. When a file or message send it will get delivered unless connections fails. If connection lost, the server will request the lost part. There is no corruption while transferring a message.	|Reliability: UDP is connectionless protocol. When you a send a data or message, you don't know if it'll get there, it could get lost on the way. There may be corruption while transferring a message.
|Ordered: If you send two messages along a connection, one after the other, you know the first message will get there first. You don't have to worry about data arriving in the wrong order.|Ordered: If you send two messages out, you don't know what order they'll arrive in i.e. no ordered
|Heavyweight: - when the low level parts of the TCP "stream" arrive in the wrong order, resend requests have to be sent, and all the out of sequence parts have to be put back together, so requires a bit of work to piece together.	| Lightweight: No ordering of messages, no tracking connections, etc. It's just fire and forget! This means it's a lot quicker, and the network card / OS have to do very little work to translate the data back from the packets.
|Streaming: Data is read as a "stream," with nothing distinguishing where one packet ends and another begins. There may be multiple packets per read call.	|Datagrams: Packets are sent individually and are guaranteed to be whole if they arrive. One packet per one read call.
|Examples: World Wide Web (Apache TCP port 80), e-mail (SMTP TCP port 25 Postfix MTA), File Transfer Protocol (FTP port 21) and Secure Shell (OpenSSH port 22) etc. |Examples: Domain Name System (DNS UDP port 53), streaming media applications such as IPTV or movies, Voice over IP (VoIP), Trivial File Transfer Protocol (TFTP) and online multiplayer games etc