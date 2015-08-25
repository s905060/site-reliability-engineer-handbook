#Fdisk

Following are the 5 typical actions (commands) that you can execute inside fdisk.
* n – New Partition creation
* d – Delete an existing partition
* p - Print Partition Table
* w – Write the changes to the partition table. i.e save.
* q – Quit the fdisk utility

Create a partition
In the following example, I created a /dev/sda1 primary partition.
```
# fdisk /dev/sda Command (m for help): p
Disk /dev/sda: 287.0 GB, 287005343744 bytes
255 heads, 63 sectors/track, 34893 cylinders
Units = cylinders of 16065 * 512 = 8225280 bytes
Device Boot  Start   End      Blocks   Id  System
Command (m for help): n Command action
e extended
p   primary partition (1-4)
p
Partition number (1-4): 1
First cylinder (1-34893, default 1):
Using default value 1
Last cylinder or +size or +sizeM or +sizeK (1-34893,
default 34893):
Using default value 34893
Command (m for help): w
The partition table has been altered!
Calling ioctl() to re-read partition table.
Syncing disks.
```

Verify that the partition got created successfully
```
# fdisk /dev/sda Command (m for help): p
Disk /dev/sda: 287.0 GB, 287005343744 bytes
255 heads, 63 sectors/track, 34893 cylinders
Units = cylinders of 16065 * 512 = 8225280 bytes
Device Boot  Start         End      Blocks   Id  System
/dev/sda1        1       34893   280277991   83  Linux
Command (m for help): q
```

```
mkfs.ext4 /dev/sda1
```