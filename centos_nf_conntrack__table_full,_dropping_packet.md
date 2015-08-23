#  CentOS: nf_conntrack: table full, dropping packet.

```
# cat /etc/centos-release ; uname -ri
CentOS release 6.3 (Final)
2.6.32-279.11.1.el6.x86_64 x86_64
```
```
# dmesg
nf_conntrack: table full, dropping packet.
nf_conntrack: table full, dropping packet.
nf_conntrack: table full, dropping packet.
```

You can check how many sessions are opend from /proc/net/nf_conntrack.
```
# head -1 /proc/net/nf_conntrack
ipv4     2 udp      17 171 src=192.168.11.1 dst=192.168.11.2 sport=45669 dport=53 src=192.168.11.2 dst=192.168.11.1 sport=53 dport=45669 [ASSURED] mark=0 secmark=0 use=2
```
```
# wc -l /proc/net/nf_conntrack
34775 /proc/net/nf_conntrack
```
```
increase # of nf_conntrack_max. ( default is 65535 )
# cat /proc/sys/net/nf_conntrack_max
65535
```

Please note that the directory path to “nf_conntrack_max” would differ from Linux distributions , versions.

increase the # of  “nf_conntrack_max”
```
# echo 100000 > /proc/sys/net/nf_conntrack_max
# cat /proc/sys/net/nf_conntrack_max
100000
```

permanently set the value.
```
# tail -1 /etc/sysctl.conf
net.nf_conntrack_max = 100000
```