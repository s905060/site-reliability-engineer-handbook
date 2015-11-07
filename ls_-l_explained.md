# ls -l explained

There is a command I use a lot and it is ls -l

The -l switch turns on long listing format

Here is an example of its output:
```
drwxr-xr-x 2 root    root      4096 Mar  9 11:49 modprobe.d
-rw-r--r-- 1 root    root         0 Jan 11  2009 motd
drwxr-xr-x 2 root    root      4096 Feb 23 17:17 mplayer
-rw-r--r-- 1 root    root       311 Mar 31 10:01 mtab
-rw------- 1 root    ggarron      0 Feb 24 18:07 mtab.fuselock
-rw-r--r-- 1 root    root      2614 Jul 13  2009 mtools.conf
drwxr-xr-x 2 root    root      4096 Mar  9 11:48 mysql
-rw-r--r-- 1 root    root      8728 Feb 13 14:30 nanorc
-rw-r--r-- 1 root    root       767 Jan  4 04:40 netconfig
drwxr-xr-x 3 root    root      4096 Feb 23 17:17 nginx
-rw-r--r-- 1 root    root      2147 Jan 29  2009 nscd.conf
-rw-r--r-- 1 root    root       223 Jul 17  2009 nsswitch.conf
-rw-r--r-- 1 root    root      1451 Jun 19  2009 ntp.conf
-rw-r--r-- 1 root    root       415 Nov 13 19:47 ntpd.conf
-rw-r--r-- 1 root    root         0 Jun 18  2009 odbc.ini
-rw-r--r-- 1 root    root         0 Jun 18  2009 odbcinst.ini
drwxr-xr-x 2 root    root      4096 Feb 23 17:10 openldap
-rw-r--r-- 1 root    root      2408 Nov 10 20:05 pacman.conf
drwxr-xr-x 2 root    root      4096 Feb 23 17:18 pacman.d
drwxr-xr-x 2 root    root      4096 Mar  9 11:52 pam.d
drwxr-xr-x 2 root    root      4096 Dec 29 10:40 pango
-rw-r--r-- 1 root    root       737 Jun 26  2009 passwd
-rw------- 1 root    root       681 Jun 12  2009 passwd-
drwxr-xr-x 2 root    root      4096 Nov  2 16:38 pcmcia
drwxr-xr-x 3 root    root      4096 Mar  9 11:52 php
drwxr-xr-x 5 root    root      4096 Jan  7 12:44 pm
drwxr-xr-x 2 root    root      4096 Aug 21  2009 polipo
drwxr-xr-x 5 root    root      4096 Jan 31 06:37 polkit-1
drwxr-xr-x 5 root    root      4096 Feb 23 17:18 ppp
-rw-r--r-- 1 root    root       209 Mar 30 17:41 printcap
drwxrwx--- 3 privoxy privoxy   4096 Feb 23 17:18 privoxy
```

Let’s take this one to analyse
```
-rw-r--r-- 1 root    root       209 Mar 30 17:41 printcap
```

We will split the output for better understanding.


| Field 1 |	Field 2|	Field 3 |	Field 4|	Field 5	|Field 6|	Field 7	|Field 8|	Field 9	|Field 10|
|-- |--|--|--|--|--|--|--|--|--|
|-	|rw-|	r--	|r--|	1	|root|	root	|209|	Mar 30 17:41|	printcap

**The first field could be**

- for File, d for Directory, l for Link

**The second,third,fourth fields**

Those are permissions that means read, write and execute, and comes in three different fields that belongs to the permission the:

* second: The owner has over the file
* third: The group has over the file
* fourth: Everybody else has over the file

**The fifth field**

This field specifies the number of links or directories inside this directory.

**The sixth field is the user**

The user that owns the file, or directory

**The seventh field is te group**

The group that file belongs to, and any user in that group will have the permissions given in the third field over that file.

**The eighth field**

The size in bytes, you may modify this by using the -h option together with -l this will have the output in k,M,G for a better understanding.

**The ninth field**

The date of last modification

**The tenth field**

The name of the file

And that is it, hope you now understand better the output of ls -l command.

