# df falsely showing 100 per cent disk usage

`df` falsely says filesystem usage at 100%, and so do other applications whilst `du / -sh` shows an expected value.

>Both are correct. The difference is that whenever an application has an
open file, but the file is already deleted, then it is counted in the df
output (because the space is certainly not free) but not in du (because
it is not being used by a file).

>Just do a 'lsof | grep deleted' and I'm almost certain you'll see some
large file in /var/log being held open by some daemon that wasn't
restarted whenever it's logfile was rotated. Just restart that daemon
(and don't forget to fix the logrotate scripts for that daemon and/or
file a bugreport about it)

>My first guess is you have deleted some files that are stil open. DU will
not show the deleted files, but since they are still in use DF will count
them.
 

Sure enough this was the case. I restarted mysql and the server returned itself to normal.

```
mickey ~ # lsof | grep deleted 
...
mysqld     5840  mysql    5w      REG       8,17 60305288765     212995 /data/log/mysql/mysql.log (deleted)
```

Check for files on located under mount points. Frequently if you mount a directory (say a sambafs) onto a filesystem that already had a file or directories under it, you lose the ability to see those files, but they're still consuming space on the underlying disk. I've had file copies while in single user mode dump files into directories that I couldn't see except in single usermode (due to other directory systems being mounted on top of them).

See what 'df -i' says. It could be that you are out of inodes, which might happen if there are a large number of small files in that filesystem, which uses up all the available inodes without consuming all the available space.