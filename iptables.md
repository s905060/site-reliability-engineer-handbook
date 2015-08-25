# Iptables

Delete Existing Rules
Before you start building new set of rules, you might want to clean-up all the default rules, and existing rules. Use the iptables flush command as shown below to do this.
```
iptables -F
(or)
iptables --flush
```