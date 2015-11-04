# HowTo Set up hostbased authentication for passphraseless SSH communication.

### Necessary settings on the source machine

Three files on the server or target host must be modified to get host-based authentication working:
/etc/shosts.equiv - same syntax as old rhosts.equiv, can point to netgroups
/etc/ssh/ssh_known_hosts - hold the identities of the clients
/etc/ssh/sshd_config - turn on host-based authentication

Adjustments to ssh and sshd have to be done at two locations. First the source machine setup will be described:
The file /etc/ssh/ssh_config must contain the following two lines:
```
HostbasedAuthentication yes
EnableSSHKeysign yes
```
The file /etc/ssh/ssh_known_hosts needs to include a line for each machine you want to connect to to avoid that the user’s personal ~/.ssh/known_hosts file will be created at all. This entry is usually entered automatically in the user’s ~/.ssh/known_hosts file and looks like:
```
node01,192.168.0.1 ssh-rsa AAAAB3NzaC1yc2EAAA...
```
Both entries (the hostname which you use to connect, and its TCP/IP address) must be recorded there as these are cross-checked by SSH’s protocol. In case that you installed your cluster with an identical image on all nodes, you can also use wildcards like node* or 192.168.0.* there, so that all nodes in the cluster can be represented by a single line.

In case that each node has an unique entry, all the keys can be collected by the command ssh-keyscan, which you can use in a loop across the complete cluster.

One file needs to run as root, but it isn’t installed by default on Linux to include the SUID bit in some distributions, so you have to execute:
```
$ chmod u+s /usr/lib64/ssh/ssh-keysign
```
The location of the file may vary on your system (e.g. /usr/libexec/openssh on RHEL and /usr/lib/openssh on Debian).



---


### Necessary settings on the target machine
Several files exit in the directory /etc to define the machines which are eligible to connect to this one, depending on the protocol used for the connection:
In case you want to allow RSH connection with qrsh from the headnode of the cluster, the name of the headnode can be added to /etc/hosts.equiv. This is not directly necessary for the hostbased SSH setup, as it will use a different list, but as this list is also honored by sshd while looking for allowed hosts, it’s mentioned here. Often only the name of the headnode is in this list.

The file /etc/ssh/shosts.equiv needs to include a line for each machine you want to allow connections from. For hostbased SSH authentication inside the cluster this file should contain all hostnames of the nodes and exist on all nodes.

The hostbased authentication must be enabled for the sshd running on the target machine by an entry in /etc/ssh/sshd_config:
```
HostbasedAuthentication yes
```
It is necessary to reload/restart the sshd after these changes, to make him aware of the changed configuration. It’s also possible to disable the password based login at this time.

The file /etc/ssh/ssh_known_hosts needs to include a line for each machine you want to allow connections from. As already discribed for the setup of the source machine, its entries look like:
```
node01,192.168.0.1 ssh-rsa AAAAB3NzaC1yc2EAAA...
```
This is not coincidence, that this is the same file which is also used on the source machine. I.e. one and the same file is used at two locations. On the source machine it will check whether the target machine is already known (which each user usually notice as and added hostkey to his personal ~/.ssh/known_hosts otherwise as ssh will check whether it was changed and/or a man-in-the-middle-attack happens), and on the target machine it is used to contain entries for the allowed source machines.

As the sshd on the target machine will try to perform a reverse name resolution (i.e. get the TCP/IP address out of the name and then the FQDN out of the TCP/IP address), it’s necessary that each record contains all three entries: hostname, FQDN, TCP/IP address. If a FQDN is not defined like in a private cluster where you define only hostnames without a domain in /etc/hosts, it is sufficient to put only hostname and TCP/IP address therein though.

Newer versions of openSSH also allow the use of wildcards in the hostname and TCP/IP address. In a uniform cluster where all nodes are replicated and have the same hostkey in /etc/ssh it is this way sufficient to have only one entry there like:
```
node*,192.168.1.* ssh-rsa AAAAB3NzaC1yc2EAAA...
```
If you want to allow hostbased authentication also for the account of the root user, it is necessary to put the hostname of  all allowed hosts in a file called /root/.shosts on all machines. As root’s home is usually not shared in a cluster, this must be copied to each machine.


---


### Side notes
Often it is suggested, to keep the entries in ~/.ssh/known_hosts hashed (this can be done once by ssh-keygen -H [or automatically for each new entry]; further access for manipulation of this file can then be achived by commands like ssh-keygen -F hostname or ssh-keygen -R hostname), so that an intruder wouldn't know the machines to which you connect to usually. But as these logins are also recorded most likely in the ~/.bash_history, it would imply to disable the recording of such commands like ssh and scp by a line like:
```
export HISTIGNORE="ssh*:scp*"
```
in your setup in one of the files the Bash sources during startup. Having done so, there is another pitfall: the file ~/.ssh/config. This is often used in case you want to use abbrevations to your login destinations, while also using an alternative port and/or login name for certain destinations can be defined there. The file may look like:
```
Host fast
    User my_surname
    Port 455
    Hostname fast.server.site.demo

Host default
    Hostname machine.local.demo

Host node*
    ForwardX11 no
    ForwardX11Trusted no

Host *
    ForwardAgent yes
    ForwardX11 yes
    ForwardX11Trusted yes
    Compression yes
    NoHostAuthenticationForLocalhost yes
    ServerAliveInterval 900 
```
When you decide to use such a handy setup, there seems to be no way to hide this information from an intruder. If you want to use this in a cluster as general setup, at least the last two blocks could better be placed in the common /etc/ssh/ssh_config where they will be honored by all users by default

Although it’s not in the range of this Howto, the use of an ssh-agent running on your local machine may ease the connection to several remote clusters and even allow copying between two remote clusters without the need of entering any authentication credentials. A  good explanation of the underlying machanism can be found at http://unixwiz.net/techtips/ssh-agent-forwarding.html