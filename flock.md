# Flock in crontab

```
* * * * * /usr/bin/flock -n /tmp/fcj.lockfile /usr/local/bin/frequent_cron_job --minutely
```