# Howto Install and Configure Cobbler on Centos 6

This document describes the steps to get cobbler up and running on Centos 6
After the installation finnishes we need to perform the following steps before we can install Cobbler:

1. Configure the network interfaces 2. Configure the resolver
3. Disable Selinux
4. Disable Iptables Firewall
5. Install EPEL Yum Repository

###Configure Linux
###Configure the network interfaces
In Redhat 6 Network configurations are handled by network-manager. Since I always install a minimalistic server network-manager is by default not installed and configuration should be done the old fashioned way by edditing the configuration files directly.
First we set the correct hostname and gateway

```
vi /etc/sysconfig/network
```

###Configure the resolver
###Disable selinux
Change the following line from:
```
SELINUX=enforcing

```
to:

```
SELINUX=disabled
```

###Disable iptables
Stop Iptables:
```
service iptables stop
```

Disable iptables:
```
chkconfig iptables off
```

###Install the Fedora EPEL repository
```
rpm -Uhv
http://download.fedora.redhat.com/pub/epel/6/i386/epel-release-6-5.noarch.rp
m
```

###Install Cobbler and prerequisites
Install the required packages:
```
yum install cobbler cobbler-web pykickstart
```

Enable the required services:
```
chkconfig httpd on
chkconfig xinetd on
chkconfig cobblerd on
```

Start the required services:
```
service httpd start
service xinetd start
service cobblerd start
```

###Configure Cobbler and prerequisites 
###Install and Configure ISC Dhcp and Bind

For a complete standalone install server we need to install a local dhcp server and a dns server, we will use the bind dns software and the dhcp software both from ISC. Bind has a track record of being hard to configure because of the miriad of options but since cobbler does all the configuring for us we dont need to worry about that.

First install Bind and Dhcp
```
yum install -y bind bind-utils dhcp
```

Enable Bind
```
chkconfig named on
```