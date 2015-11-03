# Hot-To Linux Hard Disk Format Command

Q. I've installed a new 250GB SATA hard disk on our office CentOS Linux server. How do I format a hard disk under Linux operating system from a shell prompt?

A.. There are total 4 steps involved for hard disk upgrade and installation procedure:

### Step #1 : Partition the new disk using fdisk command

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
* m - print help
* p - print the partition table
* n - create a new partition
* d - delete a partition
* q - quit without saving changes
* w - write the new partition table and exit

### Step#2 : Format the new disk using mkfs.ext3 command

To format Linux partitions using ext2fs on the new disk:
```
# mkfs.ext3 /dev/sdb1
```

### Step#3 : Mount the new disk using mount command

First create a mount point /disk1 and use mount command to mount /dev/sdb1, enter:
```
# mkdir /disk1
# mount /dev/sdb1 /disk1
# df -H
```

### Step#4 : Update /etc/fstab file

Open /etc/fstab file, enter:
```
# vi /etc/fstab
```

Append as follows:
```
/dev/sdb1               /disk1           ext3    defaults        1 2
```
Save and close the file.