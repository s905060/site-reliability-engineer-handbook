# Write message to a syslog / log file

**syslog** is the protocol as well as application to send message to Linux system logfile located at /var/log directory.

Sysklogd provides two system utilities which provide support for system logging and kernel message trapping.

Usually most program and apps use C or syslog application / library sending syslog messages.

But how do you send message from a shell prompt or shell script?

**logger command**
Use logger command which is a shell command interface to the syslog system log module. It makes or writes one line entries in the system log file from the command line.

Log message System rebooted for hard disk upgrade

```$ logger System rebooted for hard disk upgrade```

You can see message in /var/log/message file

```# tail -f /var/log/message```

Output:
```
Jan 26 20:53:31 dell6400 logger: System rebooted for hard disk upgrade
```

You can use logger command from a shell script. Consider following example:

```
#!/bin/bash
HDBS="db1 db2 db3 db4"
BAK="/sout/email"
[ ! -d $BAK ] && mkdir -p $BAK || :
/bin/rm $BAK/*
NOW=$(date +"%d-%m-%Y")
ATTCH="/sout/backup.$NOW.tgz"
[ -f $ATTCH ] && /bin/rm $ATTCH  || :
MTO="you@yourdomain.com"
for db in $HDBS
do
 FILE="$BAK/$db.$NOW-$(date +"%T").gz"
 mysqldump -u admin -p'password' $db | gzip -9 > $FILE
done
tar -jcvf $ATTCH $BAK
 mutt -s "DB $NOW" -a $ATTCH $MTO <<EOF
DBS $(date)
EOF
[ "$?" != "0" ] &&  logger "$0 - MySQL Backup failed" || :
```