# Logrotate

```
# sample logrotate configuration file
       compress

       /var/log/messages {
           rotate 5
           weekly
           postrotate
                                     /sbin/killall -HUP syslogd
           endscript
       }

       "/var/log/httpd/access.log" /var/log/httpd/error.log {
           rotate 5
           mail www@my.org
           size=100k
           sharedscripts
           postrotate
                                     /sbin/killall -HUP httpd
           endscript
       }

       /var/log/news/news.crit {
           monthly
           rotate 2
           olddir /var/log/news/old
           missingok
           postrotate
                                     kill -HUP ‘cat /var/run/inn.pid‘
           endscript
           nocompress
       }
```