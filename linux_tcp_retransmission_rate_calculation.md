# Linux TCP retransmission rate calculation

TCP retransmission rate is a manifestation of the network quality, simple packaging netstat -s output can be calculated TCP retransmission rate. Ready-made script as follows:

```
#!/bin/bash
export PATH='/bin:/sbin:/usr/bin:/usr/sbin:/usr/local/bin:/usr/local/sbin'
SHELLDIR="$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )"

netstat -s -t > /tmp/netstat_s 2>/dev/null

s_r=`cat /tmp/netstat_s | grep 'segments send out' | awk '{print $1}'`
s_re=`cat /tmp/netstat_s  | grep 'segments retransmited' | awk '{print $1}'`

[ -e ${SHELLDIR}/s_r ] || touch ${SHELLDIR}/s_r
[ -e ${SHELLDIR}/s_re ] || touch ${SHELLDIR}/s_re

l_s_r=`cat ${SHELLDIR}/s_r`
l_s_re=`cat ${SHELLDIR}/s_re`

echo $s_r > ${SHELLDIR}/s_r
echo $s_re > ${SHELLDIR}/s_re

tcp_re_rate=`echo "$s_r $s_re $l_s_r $l_s_re" | awk '{printf("%.2f",($2-$4)/($1-$3)*100)}'`
echo $tcp_re_rate
```

Possible causes high rate of TCP retransmissions
Description Network transmission occurs retransmission packet loss, basically from three points to locate: the client network, the service side of the network, the intermediate link network conditions

* Client machine network anomalies
* Traffic run over the server NIC, NIC packet loss, attention ifconfig the error output
* Intermediate network connection road congestion, such as the switch-linked core switch links, etc., required to check each link traffic situation