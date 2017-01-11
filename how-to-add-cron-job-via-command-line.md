```bash
crontab -l > file; echo '*/5 * * * * /usr/local/bin/nscawrap `hostname` scylla-server_status /usr/lib/nagios/plugins/check_procs -c 1:1 -u scylla -C scylla' >> file; crontab file

crontab -l > file; echo '*/5 * * * * /usr/local/bin/nscawrap `hostname` scylla-jmx_status /usr/lib/nagios/plugins/check_procs -c 1:1 -u scylla -C scylla-jmx' >> file; crontab file
```