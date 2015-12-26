# How to determine OOM

OOM (Out of Memory) machines stiff and die, the application is the greatest harm, so we need to have an effective means of monitoring to determine whether the machine is OOM.

OOM machine features

* Machine can ping
* The machine can not ssh, but can telnet 22 port, but can not ssh upswing

Guess OOM state of the machine

* Linux's kernel might still alive, at least tcp / ip protocol stack will still work, but also because the ping ip, port 22 is still listening
* Do not respond to the upper application, such as telnet on port 22 can be connected, but no response when entering any character

OOM monitor script
```
#/bin/bash
ips='
192.168.1.1
192.168.1.2
192.168.1.3
'
for i in $ips
do
    #Check if pingable
    if ping -c2 -w5 $i 2>/dev/null ; then
        # Use nc to check port 22,nc -w set nc timeout
        if echo 'hello' | nc -w 10 $i 22 | grep -q 'OpenSSH' ; then
            echo "$ip is OK"
        else
            echo "$ip is OOM!"
        fi
    fi
done
```