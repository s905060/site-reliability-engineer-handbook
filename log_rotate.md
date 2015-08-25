# Log Rotate

This hack explains how to rotate the apache access_log and error_log files.
Add the following file to /etc/logrotate.d directory.

```
# vi /etc/logrotate.d/apache
/usr/local/apache2/logs/access_log
/usr/local/apache2/logs/error_log {
    size 100M
    compress
    dateext
    maxage 30
    postrotate
      /usr/bin/killall -HUP httpd
      ls -ltr /usr/local/apache2/logs | mail -s
"$HOSTNAME: Apache restarted and log files rotated"
ramesh@thegeekstuff.com
endscript }
```

Note: Refer to our logrotate tutorial (with 15 examples) that explains more details about how to use logrotate options.
In the above /etc/logrotate.d/apache example:
* size 100M – Once the access_log, and error_log reaches 100M, it will be rotated. You can also use 100k (for Kb), 100G (for GB). Instead of size, you can also rotate apache logs using frequency (daily, weekly, monthly).
* compress – Indicates that the rotated log file will be compressed. By default this uses gzip. So, the rotated file will have .gz extension.
* dateext - Appends the date in YYYYMMDD format to the rotated log files. i.e Instead of access_log.1.gz, it creates access_log- 20110616.gz
* maxage - Indicates how long the rotated log files should be kept. In this example, it will be kept for 30 days.
* postrotate and endscript – Any commands enclosed between these two parameter will be executed after the log is rotated.

**Important: Once you rotate the log files, you want apache to write the new log messages to the newly created access_log and error_log. So, you need to send the HUP signal to the apache as shown here. Make sure to do /usr/bin/killall -HUP httpd, which will restart the apache after rotating the log files (Read more about kill).**

Also, you might want to send an email to yourself indicating that the log file is rotated, along with the output of ls -ltr command as the body of the email. i.e Add the following between “postrotate” and “endscript” option (after the killall command).

```
ls -ltr /usr/local/apache2/logs | mail -s "$HOSTNAME:
Apache restarted and log files rotated"
ramesh@thegeekstuff.com
```

The /etc/cron.daily/logrotate script runs everyday that will perform log rotate of all the files as specified in the /etc/logrotate.conf and all the file under /etc/logrotate.d directory.

After adding the above /etc/logrotate.d/apache file, for testing purpose, you can manually call the logrotate script as shown below.

```
# /etc/cron.daily/logrotate
```