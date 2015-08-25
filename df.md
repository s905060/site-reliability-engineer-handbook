# Df

df command (disk free) displays the amount of total and free disk space available on the mounted filesystems.
```
Syntax: df [options] [name]
```

How much GB of disk space is free on my system?
Use df -h as shown below. Option -h displays the values in human readable format (for example: K for Kb, M for Mb and G for Gb). In the sample output below, / filesystem has 17GB of disk space available and /home/user filesystem has 70GB available.
```
# df –h
Filesystem            Size  Used Avail Use% Mounted on
/dev/sda1             64G   44G   17G  73%   /
/dev/sdb1            137G   67G   70G  49%   /home/user
```

What type of filesystem do I have on my system?
Option -T will display the information about the filesystem Type. In this example / and /home/user filesystems are ext2. Option -a will display all the filesystems, including the 0 size special filesystem used by the system.
```
# df -Tha
Filesystem    Type     Size  Used  Avail Use%  Mounted on
/dev/sda1    ext2    64G    44G   17G  73%    /
/dev/sdb1    ext2   137G    67G   70G  49%    /home/user
none         proc      0      0     0    -    /proc
none        sysfs      0      0     0    -    /sys
none       devpts      0      0     0    -    /dev/pts
none        tmpfs   2.0G      0  2.0G    0%   /dev/shm
```