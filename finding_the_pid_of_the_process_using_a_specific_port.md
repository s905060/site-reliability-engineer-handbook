# Finding the PID of the process using a specific port?
```
$ sudo netstat -nlp | grep 80
tcp  0  0  0.0.0.0:80  0.0.0.0:*  LISTEN  125004/nginx
```

```
# lsof -i :25
COMMAND  PID        USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
exim4   2799 Debian-exim    3u  IPv4   6645      0t0  TCP localhost:smtp (LISTEN)
exim4   2799 Debian-exim    4u  IPv6   6646      0t0  TCP localhost:smtp (LISTEN)
```