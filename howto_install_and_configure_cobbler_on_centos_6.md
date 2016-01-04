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