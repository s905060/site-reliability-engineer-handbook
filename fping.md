# fping

fping is one my favorite network profiling / scripting tool. It uses the Internet Control Message Protocol (ICMP) echo request to determine if a target host is responding or not.

Unlike ping , fping is meant to be used in scripts, so its output is designed to be easy to parse.

You can easily write perl / shell script to check a list of hosts and send mail if any are unreachable.

fping command example

Just type the following command to see if we can reach to router:

`$ fping router`

Output:

`router is alive`

You can read list of targets (hosts / servers) from a file. The -f option can only be used by the root user. Regular users should pipe in the file via
I/O redirectors (stdin). For example read all host names from ~/.ping.conf file

`$ fping < ~/.ping.conf`

You can also netmask to ping the entire network i.e generate a target list from a supplied IP netmask. For example, ping the class C 192.168.1.x:

`$ fping -g 192.168.1.0/24`

or

`$ fping -g 192.168.1.0 192.168.1.255`

Sample shell script to send email if host is down
```
#!/bin/bash
HOSTS="router sun printer laptop sony-laptop xbox backup-server"
DLIST=""
for h in $HOSTS
do
  fping -u $h >& /dev/null
  if [ $? -ne 0 ]; then
          echo ${h} host is down send email
          # mail -s "Host ${h} down" admin@you.com </dev/null
  fi
done
```

Another good example is when you want to perform an action only on hosts that are currently reachable.
```
#!/usr/bin/perl
$myHosts = ‘cat /etc/hosts.backup | fping -a‘;
foreach $host (split(/\n/,$myHosts)) {
        # take action or call other function
}
```