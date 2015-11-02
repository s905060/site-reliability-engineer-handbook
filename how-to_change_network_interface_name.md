# How-To Change Network Interface Name

The best way to rename a network interface is through udev.

Edit the file /etc/udev/rules.d/70-persistent-net.rules to change the interface name of a network device.

The names of the network devices are listed in this file as follows :
```
# PCI device 0x11ab:0x4363 (sky2)
SUBSYSTEM=="net", ACTION=="add", DRIVERS=="?*",
ATTR{address}=="00:00:00:00:00:00",ATTR{dev_id}=="0x0", ATTR{type}=="1",
KERNEL=="eth*", NAME="eth0"
```

### Rename network interface from eth0 to wan0

To rename interface eth0 to wan0, edit /etc/udev/rules.d/70-persistent-net.rules file and change NAME="eth0" to NAME="wan0".
```
# PCI device 0x11ab:0x4363 (sky2)
SUBSYSTEM=="net", ACTION=="add", DRIVERS=="?*",
ATTR{address}=="00:00:00:00:00:00",ATTR{dev_id}=="0x0", ATTR{type}=="1",
KERNEL=="eth*", NAME="wan0"
```

For Centos/RHEL etc.
Rename the network interface configuration file :
```
# cd etc/sysconfig/network-scripts/
# mv ifcfg-eth0 ifcfg-wan0
```

Edit the network interface configuration file and replace all occurrences of the old name eth0 with the new one wan0 :
```
# vi /etc/sysconfig/network-scripts/ifcfg-wan0
```

For Ubuntu etc.
Edit the /etc/network/interfaces file and replace all occurrences of the old name eth0 with the new one wan0 :
```
# sudo nano /etc/network/interfaces
```

Test changes
Reboot the system to test changes :
```
# reboot
```

Verify new settings :
```
# ifconfig -a
```

###Rename network interface from eth1 back to eth0
>Q : Why does my network interface name change?

>A : The interface name of a network device increases if the MAC address of a network card changes.

Edit the file /etc/udev/rules.d/70-persistent-net.rules.

Copy the new MAC address from eth1 to the line of your eth0 rule.

Delete the rule for eth1. Save and close the file.

For Centos/RHEL etc.
Check the network interface configuration located under :
```
# vi /etc/sysconfig/network-scripts/ifcfg-eth0
```

Don't forget to replace the old MAC address with the new one.

For Ubuntu etc.
Make sure /etc/network/interfaces file has correct configuration :
```
# sudo nano /etc/network/interfaces
```

Test changes
Reboot the system to test changes :
```
# reboot
```

Verify new settings :
```
# ifconfig -a
```