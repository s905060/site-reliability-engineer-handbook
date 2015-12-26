# Ttroubleshooting network issues

When troubleshooting network issues from host A to host B, check the following:

###Do you have connectivity?

Ensure network interface has an IP with /sbin/ifconfig
Network interfaces are configured at: /etc/sysconfig/network-scripts/ifcfg-<interface>

If you modify the config restart with: sudo service network restart

###Does it have a path to the local network?

Check if default gateway is present:
```
[vagrant@centos-5 ~]$ /sbin/route -n
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
10.0.2.0        0.0.0.0         255.255.255.0   U     0      0        0 eth0
192.168.33.0    0.0.0.0         255.255.255.0   U     0      0        0 eth1
169.254.0.0     0.0.0.0         255.255.0.0     U     0      0        0 eth1
0.0.0.0         10.0.2.2        0.0.0.0         UG    0      0        0 eth0
```

Note: the -n option stops the command from attempting to resolve the IPs to hostname

Ping the gateway If you can’t ping it could just mean ICMP packets have been blocked. or (If ICMP isn’t blocked) your switch port on your host could be set to the wrong VLAN.

###Check DNS

Perform a nslookup to check if it resolves the IP.
If a name server is not configured check /etc/resolv.conf

###Connectivity Matrix
```
|-- can_ping_host
|   `-- not_responding
|       `-- is_the_remote_port_open?
`-- cannot_ping_host
    |-- different_subnet?
    |   `-- is_there_a_route_to_remote_host?
    `-- same_subnet?
        `-- is_the_nameserver_down?
```

###Is there a route to the remote host?

Perform a traceroute. If ping works but traceroute doesn’t it could be because UDP is blocked.

###Is the port open?

Perform a telnet test. If it fails either the remote machine is not listening on that port or the firewall is blocking it.
Check if the port on the remote machine is open by running nestat locally on the remote machine:

`netstat -nlp`

If the port is blocked by firewall check the firewall rule with iptables

###Packet Captures

Suitable to investigate intermittent issues

```
# See traffic coming from a host
sudo /usr/sbin/tcpdump -nA host [source-host]

# Filter traffic to and from a specific port
sudo /usr/sbin/tcpdump -n port [port-number]

# Multiple ports
sudo /usr/sbin/tcpdump -n port [port-number] or [port-number]

# Write to file and to standard output
sudo /usr/sbin/tcpdump -l host [host] | tee outputfile
```
