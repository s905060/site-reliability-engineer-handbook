# Crontab

Setting up cron jobs in Unix and Solaris
cron is a unix, solaris utility that allows tasks to be automatically run in the background at regular intervals by the cron daemon. These tasks are often termed as cron jobs in unix , solaris.  Crontab (CRON TABle) is a file which contains the schedule of cron entries to be run and at specified times.
This document covers following aspects of Unix cron jobs

1. Crontab Restrictions
2. Crontab Commands
3. Crontab file – syntax
4. Crontab Example
5. Crontab Environment
6. Disable Email
7. Generate log file for crontab activity

#### Run cron manually
```
run-parts /etc/cron.hourly
```

### Crontab Restrictions
You can execute crontab if your name appears in the file /usr/lib/cron/cron.allow. If that file does not exist, you can use crontab if your name does not appear in the file /usr/lib/cron/cron.deny.
If only cron.deny exists and is empty, all users can use crontab. If neither file exists, only the root user can use crontab. The allow/deny files consist of one user name per line.

### Crontab Commands
export EDITOR=vi ;to specify a editor to open crontab file.
crontab -e    Edit your crontab file, or create one if it doesn’t already exist.
crontab -l      Display your crontab file.
crontab -r      Remove your crontab file.
crontab -v      Display the last time you edited your crontab file. (This option is only available on a few systems.)

### Crontab file
Crontab syntax :
A crontab file has five fields for specifying day , date and time followed by the command to be run at that interval.

```
*     *     *   *    *        command to be executed
-     -     -   -    -
|     |     |   |    |
|     |     |   |    +----- day of week (0 - 6) (Sunday=0)
|     |     |   +------- month (1 - 12)
|     |     +--------- day of        month (1 - 31)
|     +----------- hour (0 - 23)
+------------- min (0 - 59)
```

* in the value field above means all legal values as in braces for that column.
The value column can have a * or a list of elements separated by commas. An element is either a number in the ranges shown above or two numbers in the range separated by a hyphen (meaning an inclusive range).
Notes

A. ) Repeat pattern like /2 for every 2 minutes or /10 for every 10 minutes is not supported by all operating systems. If you try to use it and crontab complains it is probably not supported.
B.) The specification of days can be made in two fields: month day and weekday. If both are specified in an entry, they are cumulative meaning both of the entries will get executed .

### Crontab Example
A line in crontab file like below removes the tmp files from /home/someuser/tmp each day at 6:30 PM.
```
30     18     *     *     *         rm /home/someuser/tmp/*
```

Changing the parameter values as below will cause this command to run at different time schedule below :
```
min	hour	day/month	month	day/week	Execution time
30	0	1	1,6,12	*	– 00:30 Hrs  on 1st of Jan, June & Dec.
0	20	*	10	1-5	–8.00 PM every weekday (Mon-Fri) only in Oct.
0	0	1,10,15	*	*	– midnight on 1st ,10th & 15th of month
5,10	0	10	*	1	– At 12.05,12.10 every Monday & on 10th of every month

```

Note : If you inadvertently enter the crontab command with no argument(s), do not attempt to get out with Control-d. This removes all entries in your crontab file. Instead, exit with Control-c.

### Crontab Environment
cron invokes the command from the user’s HOME directory with the shell, (/usr/bin/sh).
cron supplies a default environment for every shell, defining:
```
HOME=user’s-home-directory
LOGNAME=user’s-login-id
PATH=/usr/bin:/usr/sbin:.
SHELL=/usr/bin/sh
```

Users who desire to have their .profile executed must explicitly do so in the crontab entry or in a script called by the entry.

### Disable Email
By default cron jobs sends a email to the user account executing the cronjob. If this is not needed put the following command At the end of the cron job line .
```
>/dev/null 2>&1
```

### Generate log file
To collect the cron execution execution log in a file :
```
30 18 * * * rm /home/someuser/tmp/* > /home/someuser/cronlogs/clean_tmp_dir.log
```

Crontab examples
1. Run at 12:01 a.m. 1 minute after midnight everyday. This is a good time to run backup when the system is not under load.
```
1 0 * * * /root/bin/backup.sh
```
2. Run backup every weekday (Mon – Fri) at 11:59 p.m.
```
59 11 * * 1,2,3,4,5 /root/bin/backup.sh
```
Following will also do the same.
```
59 11 * * 1-5 /root/bin/backup.sh
```
3. Execute the command every 5 minutes.
```
*/5 * * * * /root/bin/check-status.sh
```
4. Execute at 1:10 p.m on 1st of every month
```
10 13 1 * * /root/bin/full-backup.sh
```
5. Execute 11 p.m on weekdays.
```
0 23 * * 1-5 /root/bin/incremental-backup.sh
```

Crontab Options
The following are the available options with crontab:
* crontab –e : Edit the crontab file. This will create a crontab, if it doesn’t exist
* crontab –l : Display the crontab file.
* crontab -r : Remove the crontab file.
* crontab -ir : This will prompt user before deleting a crontab.