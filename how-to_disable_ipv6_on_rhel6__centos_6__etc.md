# How-To disable IPv6 on RHEL6 / CentOS 6 / etc

Proper way of disabling IPv6 subsytem in RedHat Linux 6 / CentOS 6 (dont unload modules or so)
```
in /etc/sysctl.conf  :  net.ipv6.conf.all.disable_ipv6 = 1

in /etc/sysconfig/network  : NETWORKING_IPV6=no

in /etc/sysconfig/network-scripts/ifcfg-eth0 : IPV6INIT=”no”

disable iptables6 – chkconfig –level 345 ip6tables off

reboot
```
or
```
echo “net.ipv6.conf.default.disable_ipv6=1″ >> /etc/sysctl.conf
echo “net.ipv6.conf.all.disable_ipv6 = 1″ >> /etc/sysctl.conf
```
then run the following command
```
sysctl -p
```
no reboot is required
note the space before and after the =

if you want to test the config on the fly without editing the sysctl.conf file execute the following commands
```
sysctl -w net.ipv6.conf.default.disable_ipv6=1
sysctl -w net.ipv6.conf.all.disable_ipv6=1
```