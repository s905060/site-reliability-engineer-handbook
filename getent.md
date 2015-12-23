# getent – get entries from administrative database

getent is Unix command which helps you query one of the following administrative databases in Unix: passwd, group, hosts, services, protocols, or networks.

Administrative databases in Unix

As you can probably see from their names, the administrative databases are here to help you gather the most vital information about your environment:

* passwd – can be used to confirm usernames, userids, home directories and full names of your users
* group – all the information about Unix groups known to your system
* services – all the Unix services configured on your system
* networks – networking information – what networks your system belongs to
* protocols – everything your system knows about network protocols

###How to use getent

My home PC has a hostname of ubuntu. If I ever need to double-check which IPs this hostname points to, here's how I can use getent:
```
ubuntu$ getent hosts ubuntu
127.0.1.1       ubuntu
192.168.0.2     ubuntu
```

###Using getent to find a UID by username

getent accepts various keys when searching in databases. For the passwd one, you can user either username or user id (UID) to search the database.
```
ubuntu$ getent passwd greys
greys:x:1000:1000:Gleb Reys,,,:/home/greys:/bin/bas
```

###Using getent to find a username by UID

Like I said, the opposite will work as well:
```
ubuntu$ getent passwd 1000
greys:x:1000:1000:Gleb Reys,,,:/home/greys:/bin/bash
```