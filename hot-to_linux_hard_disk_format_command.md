# Hot-To Linux Hard Disk Format Command

Q. I've installed a new 250GB SATA hard disk on our office CentOS Linux server. How do I format a hard disk under Linux operating system from a shell prompt?

A.. There are total 4 steps involved for hard disk upgrade and installation procedure:

Step #1 : Partition the new disk using fdisk command

Following command will list all detected hard disks:
```
# fdisk -l | grep '^Disk'
```
Output:
```
Disk /dev/sda: 251.0 GB, 251000193024 bytes
Disk /dev/sdb: 251.0 GB, 251000193024 bytes
```
A device name refers to the entire hard disk. For more information see Linux partition naming convention and IDE drive mappings.
To partition the disk - /dev/sdb, enter:
```
# fdisk /dev/sdb
```
The basic fdisk commands you need are:
