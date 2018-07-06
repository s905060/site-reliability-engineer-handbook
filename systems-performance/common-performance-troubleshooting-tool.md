## Common Performance Troubleshooting Tool

```
#!/bin/bash

sudo uptime

sudo dmesg -T | tail -n 30

sudo vmstat 1 5 -S m

sudo mpstat -P ALL 1 5

sudo pidstat -t 1 5

sudo iostat -xmdz 1 5

sudo sar -n TCP,ETCP,DEV 1 5

sudo free -m

sudo ss -s

sudo swapon

sudo netstat -s

sudo lsof -iTCP -sTCP:ESTABLISHED

sudo ss -mop

sudo ss –i

sudo ps -ef f

# strace –tttT –p 313
# tcpdump -i eth0 -w /tmp/out.tcpdump
# iotop
# slabtop
# sudo iotop --only -n 5
# sudo iftop -i bond0 5
```



