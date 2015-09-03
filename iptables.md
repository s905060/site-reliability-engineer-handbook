# Iptables

Delete Existing Rules
Before you start building new set of rules, you might want to clean-up all the default rules, and existing rules. Use the iptables flush command as shown below to do this.
```
iptables -F
(or)
iptables --flush
```

Set Default Chain Policies
The default chain policy is ACCEPT. Change this to DROP for all INPUT, FORWARD, and OUTPUT chains as shown below.

```
iptables -P INPUT DROP
iptables -P FORWARD DROP
iptables -P OUTPUT DROP
```
When you make both INPUT, and OUTPUT chain’s default policy as DROP, for every firewall rule requirement you have, you should define two rules. i.e one for incoming and one for outgoing.

In all our examples below, we have two rules for each scenario, as we’veset DROP as default policy for both INPUT and OUTPUT chain.

If you trust your internal users, you can omit the last line above. i.e Do not DROP all outgoing packets by default. In that case, for every firewall rule requirement you have, you just have to define only one rule. i.e define rule only for incoming, as the outgoing is ACCEPT for all packets.

Block a Specific ip-address
Before we proceed further will other examples, if you want to block a specific ip-address, you should do that first as shown below. Change the “x.x.x.x” in the following example to the specific ip-address that you like to block.
```
BLOCK_THIS_IP="x.x.x.x"
iptables -A INPUT -s "$BLOCK_THIS_IP" -j DROP
```
This is helpful when you find some strange activities from a specific ip- address in your log files, and you want to temporarily block that ip- address while you do further research.

You can also use one of the following variations, which blocks only TCP traffic on eth0 connection for this ip-address.

```
iptables -A INPUT -i eth0 -s "$BLOCK_THIS_IP" -j DROP
iptables -A INPUT -i eth0 -p tcp -s "$BLOCK_THIS_IP" -j
DROP
```

Allow ALL Incoming SSH
The following rules allow ALL incoming ssh connections on eth0 interface.
```
iptables -A INPUT -i eth0 -p tcp --dport 22 -m state
--state NEW,ESTABLISHED -j ACCEPT

iptables -A OUTPUT -o eth0 -p tcp --sport 22 -m state
--state ESTABLISHED -j ACCEPT
```

Allow Incoming SSH only from a Specific Network
The following rules allow incoming ssh connections only from 192.168.100.X network.
```
iptables -A INPUT -i eth0 -p tcp -s 192.168.100.0/24
--dport 22 -m state --state NEW,ESTABLISHED -j ACCEPT

iptables -A OUTPUT -o eth0 -p tcp --sport 22 -m state
--state ESTABLISHED -j ACCEPT
```
In the above example, instead of /24, you can also use the full subnet mask. i.e “192.168.100.0/255.255.255.0′′.

### Iptables backup and restore
```
service iptables save
iptables-save > /etc/iptables.save
iptables-restore <  /etc/sysconfig/iptables
```