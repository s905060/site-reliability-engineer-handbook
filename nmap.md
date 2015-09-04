# NMAP

What is the command to check open ports at remote machine
$nmap

1. Scan a System with Hostname and IP Address
The Nmap tool offers various methods to scan a system. In this example, I am performing a scan using hostname as server2.tecmint.com to find out all open ports, services and MAC address on the system.

Scan using Hostname
```
[root@server1 ~]# nmap server2.tecmint.com

Starting Nmap 4.11 ( http://www.insecure.org/nmap/ ) at 2013-11-11 15:42 EST
Interesting ports on server2.tecmint.com (192.168.0.101):
Not shown: 1674 closed ports
PORT     STATE SERVICE
22/tcp   open  ssh
80/tcp   open  http
111/tcp  open  rpcbind
957/tcp  open  unknown
3306/tcp open  mysql
8888/tcp open  sun-answerbook
MAC Address: 08:00:27:D9:8E:D7 (Cadmus Computer Systems)

Nmap finished: 1 IP address (1 host up) scanned in 0.415 seconds
You have new mail in /var/spool/mail/root
```
Scan using IP Address
```
[root@server1 ~]# nmap 192.168.0.101

Starting Nmap 4.11 ( http://www.insecure.org/nmap/ ) at 2013-11-18 11:04 EST
Interesting ports on server2.tecmint.com (192.168.0.101):
Not shown: 1674 closed ports
PORT     STATE SERVICE
22/tcp   open  ssh
80/tcp   open  http
111/tcp  open  rpcbind
958/tcp  open  unknown
3306/tcp open  mysql
8888/tcp open  sun-answerbook
MAC Address: 08:00:27:D9:8E:D7 (Cadmus Computer Systems)

Nmap finished: 1 IP address (1 host up) scanned in 0.465 seconds
You have new mail in /var/spool/mail/root
```
2. Scan using “-v” option
You can see that the below command with “-v” option is giving more detailed information about the remote machine.
```
[root@server1 ~]# nmap -v server2.tecmint.com

Starting Nmap 4.11 ( http://www.insecure.org/nmap/ ) at 2013-11-11 15:43 EST
Initiating ARP Ping Scan against 192.168.0.101 [1 port] at 15:43
The ARP Ping Scan took 0.01s to scan 1 total hosts.
Initiating SYN Stealth Scan against server2.tecmint.com (192.168.0.101) [1680 ports] at 15:43
Discovered open port 22/tcp on 192.168.0.101
Discovered open port 80/tcp on 192.168.0.101
Discovered open port 8888/tcp on 192.168.0.101
Discovered open port 111/tcp on 192.168.0.101
Discovered open port 3306/tcp on 192.168.0.101
Discovered open port 957/tcp on 192.168.0.101
The SYN Stealth Scan took 0.30s to scan 1680 total ports.
Host server2.tecmint.com (192.168.0.101) appears to be up ... good.
Interesting ports on server2.tecmint.com (192.168.0.101):
Not shown: 1674 closed ports
PORT     STATE SERVICE
22/tcp   open  ssh
80/tcp   open  http
111/tcp  open  rpcbind
957/tcp  open  unknown
3306/tcp open  mysql
8888/tcp open  sun-answerbook
MAC Address: 08:00:27:D9:8E:D7 (Cadmus Computer Systems)

Nmap finished: 1 IP address (1 host up) scanned in 0.485 seconds
               Raw packets sent: 1681 (73.962KB) | Rcvd: 1681 (77.322KB)
```
Scan Multiple Hosts

You can scan multiple hosts by simply writing their IP addresses or hostnames with Nmap.
```
[root@server1 ~]# nmap 192.168.0.101 192.168.0.102 192.168.0.103

Starting Nmap 4.11 ( http://www.insecure.org/nmap/ ) at 2013-11-11 16:06 EST
Interesting ports on server2.tecmint.com (192.168.0.101):
Not shown: 1674 closed ports
PORT     STATE SERVICE
22/tcp   open  ssh
80/tcp   open  http
111/tcp  open  rpcbind
957/tcp  open  unknown
3306/tcp open  mysql
8888/tcp open  sun-answerbook
MAC Address: 08:00:27:D9:8E:D7 (Cadmus Computer Systems)
Nmap finished: 3 IP addresses (1 host up) scanned in 0.580 seconds
```

4. Scan a whole Subnet
You can scan a whole subnet or IP range with Nmap by providing * wildcard with it.
```
[root@server1 ~]# nmap 192.168.0.*

Starting Nmap 4.11 ( http://www.insecure.org/nmap/ ) at 2013-11-11 16:11 EST
Interesting ports on server1.tecmint.com (192.168.0.100):
Not shown: 1677 closed ports
PORT    STATE SERVICE
22/tcp  open  ssh
111/tcp open  rpcbind
851/tcp open  unknown

Interesting ports on server2.tecmint.com (192.168.0.101):
Not shown: 1674 closed ports
PORT     STATE SERVICE
22/tcp   open  ssh
80/tcp   open  http
111/tcp  open  rpcbind
957/tcp  open  unknown
3306/tcp open  mysql
8888/tcp open  sun-answerbook
MAC Address: 08:00:27:D9:8E:D7 (Cadmus Computer Systems)

Nmap finished: 256 IP addresses (2 hosts up) scanned in 5.550 seconds
You have new mail in /var/spool/mail/root
```
On above output you can see that nmap scanned a whole subnet and gave the information about those hosts which are Up in the Network.

5. Scan Multiple Servers using last octet of IP address
You can perform scans on multiple IP address by simple specifying last octet of IP address. For example, here I performing a scan on IP addresses 192.168.0.101, 192.168.0.102 and 192.168.0.103.
```
[root@server1 ~]# nmap 192.168.0.101,102,103

Starting Nmap 4.11 ( http://www.insecure.org/nmap/ ) at 2013-11-11 16:09 EST
Interesting ports on server2.tecmint.com (192.168.0.101):
Not shown: 1674 closed ports
PORT     STATE SERVICE
22/tcp   open  ssh
80/tcp   open  http
111/tcp  open  rpcbind
957/tcp  open  unknown
3306/tcp open  mysql
8888/tcp open  sun-answerbook
MAC Address: 08:00:27:D9:8E:D7 (Cadmus Computer Systems)

Nmap finished: 3 IP addresses (1 host up) scanned in 0.552 seconds
You have new mail in /var/spool/mail/root
```