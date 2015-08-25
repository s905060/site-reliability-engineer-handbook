# Parted

Select the hard disk to be parted
When you execute parted command without any argument, by default it selects the first hard disk drive that is available on your system.

In the following example, it picked /dev/sda automatically as it is the first hard drive in this system.
```
# parted
GNU Parted 2.3
Using /dev/sda
Welcome to GNU Parted! Type 'help' to view a list of
commands.
(parted)
To choose a different hard disk, use the select command as
shown below.
(parted) select /dev/sdb
```

It will throw the following error message when it doesn’t find the given hard disk name.
```
Error: Error opening /dev/sdb: No medium found
Retry/Cancel? y
```

Display all Partitions Using print
Using the print command, you can view all the available partitions in the selected hard disk. The print command also displays hard disk properties such as model, size, sector size and partition table as shown below.

```
(parted) print
Model: ATA WDC WD5000BPVT-7 (scsi)
Disk /dev/sda: 500GB
Sector size (logical/physical): 512B/4096B
Partition Table: msdos
Number  Start   End     Size    Type      Filesystem Flags
 1      1049kB  106MB   105MB   primary   fat16      diag
 2      106MB   15.8GB  15.7GB  primary   ntfs       boot
 3      15.8GB  266GB   251GB   primary   ntfs
 4      266GB   500GB   234GB   extended
 5      266GB   269GB   2682MB  logical   ext4
 7      269GB   270GB   524MB   logical   ext4 
 8      270GB   366GB   96.8GB  logical   lvm
 6      366GB   370GB   3999MB  logical   linux-swap(v1)
 9      370GB   500GB   130GB   logical   ext4
```

Create Primary Partition in Selected HDD Using mkpart
mkpart command is used to create either primary or logical partition with the START and END disk locations. The below example creates partition with size around 15GB. The START and END points passed to the mkpart command are in the units of MBs.
```
(parted) mkpart primary 106 16179
```
You can also enable boot option on a partition as shown below. Linux reserves 1-4 or 1-3 partition number for primary partition and the extended partition starts from number 5.
```
(parted) set 1 boot on
```

Create Logical Partition in Selected HDD Using mkpart
Use mkpart command to create a new partition of a specific size. This will create the partition of a specific type such as primary, logical or extended without creating the file system.
Before creating the partition, execute a print command to view the current layout.
```
(parted) print
Model: ATA WDC WD5000BPVT-7 (scsi)
Disk /dev/sda: 500GB
Sector size (logical/physical): 512B/4096B
Partition Table: msdos
Number  Start   End     Size    Type      Filesystem Flags
 1      1049kB  106MB   105MB   primary   fat16      diag
 2      106MB   15.8GB  15.7GB  primary   ntfs       boot
 3      15.8GB  266GB   251GB   primary   ntfs
 4      266GB   500GB   234GB   extended
 5      266GB   316GB   50.0GB  logical   ext4
 6      316GB   324GB   7999MB  logical   linux-swap(v1)
 7      324GB   344GB   20.0GB  logical   ext4
 8      344GB   364GB   20.0GB  logical   ext2
```
Use mkpart to create a new logical partition with 127GB size as shown below.
```
(parted) mkpart logical 372737 500000
```

Execute the print command to view the new layout as shown below.
```
(parted) print
Model: ATA WDC WD5000BPVT-7 (scsi)
Disk /dev/sda: 500GB
Sector size (logical/physical): 512B/4096B
Partition Table: msdos
Number  Start   End     Size    Type      Filesystem Flags
 1      1049kB  106MB   105MB   primary   fat16      diag
 2      106MB   15.8GB  15.7GB  primary   ntfs       boot
 3      15.8GB  266GB   251GB   primary   ntfs
 4      266GB   500GB   234GB   extended
 5      266GB   316GB   50.0GB  logical   ext4
 6      316GB   324GB   7999MB  logical   linux-swap(v1)
 7      324GB   344GB   20.0GB  logical   ext4
 8      344GB   364GB   20.0GB  logical   ext2
 9      373GB   500GB   127GB   logical
 (parted)
 ```

Create a File System on Partition Using mkfs

If you use fdisk command to partition your hard disk, you need to exit the fdisk utility, and use the mkfs external program to create a file system on the partition.

However using parted utility, you can also create filesystem. Use the parted’s mkfs command to create a file system on a partition. You should be careful while doing this, as all the existing data in the partition will be lost during the file system creation. The supported filesystems in parted are ext2, mips, fat16, fat32, linux-swap, reiserfs (if libreiserfs is installed).

Let us change the file system of partition number 8 (that is shown in the print output below) from ext4 to ext2 file system.
```
(parted) print
Model: ATA WDC WD5000BPVT-7 (scsi)
Disk /dev/sda: 500GB
Sector size (logical/physical): 512B/4096B
Partition Table: msdos
Number  Start   End     Size    Type      Filesystem Flags
 1      1049kB  106MB   105MB   primary   fat16      diag
 2      106MB   15.8GB  15.7GB  primary   ntfs       boot
 3      15.8GB  266GB   251GB   primary   ntfs
 4      266GB   500GB   234GB   extended
 5      266GB   316GB   50.0GB  logical   ext4
 6      316GB   324GB   7999MB  logical   linux-swap(v1)
 7      324GB   344GB   20.0GB  logical   ext4
 8      344GB   364GB   20.0GB  logical   ext4
 9      364GB   500GB   136GB   logical   ext4
```
As shown below, use the mkfs command to change the file system type of partition number 8. mkfs command will prompt you for partition number and file system type.
```
(parted) mkfs
Warning: The existing file system will be destroyed and
all data on the partition will be lost. Do you want to
continue?
Yes/No? y
Partition number? 8
File system type?  [ext2]? ext2
```

Execute the print command again, to verify that the file system type for partition number 8 was changed to ex2.
```
(parted) print
Model: ATA WDC WD5000BPVT-7 (scsi)
Disk /dev/sda: 500GB
Sector size (logical/physical): 512B/4096B
Partition Table: msdos
Number  Start   End     Size    Type      Filesystem Flags
 1      1049kB  106MB   105MB   primary   fat16      diag
 2      106MB   15.8GB  15.7GB  primary   ntfs       boot
 3      15.8GB  266GB   251GB   primary   ntfs
 4      266GB   500GB   234GB   extended
 5      266GB   316GB   50.0GB  logical   ext4
 6      316GB   324GB   7999MB  logical   linux-swap(v1)
 7      324GB   344GB   20.0GB  logical   ext4
 8      344GB   364GB   20.0GB  logical   ext2
 9      364GB   500GB   136GB   logical   ext4
 (parted)
 ```
Create Partition and Filesystem together Using mkpartfs

Using mkpartfs parted command, you can also create a partitions with a specific filesystem. This is similar to mkpart, but with the additional feature of creating file system on a partition.

Before mkpartfs following is the layout of the partitions.
```
(parted) print
Model: ATA WDC WD5000BPVT-7 (scsi)
Disk /dev/sda: 500GB
Sector size (logical/physical): 512B/4096B
Partition Table: msdos
Number  Start   End     Size    Type      Filesystem Flags
 1      1049kB  106MB   105MB   primary   fat16      diag
 2      106MB   15.8GB  15.7GB  primary   ntfs       boot
 3      15.8GB  266GB   251GB   primary   ntfs
 4      266GB   500GB   234GB   extended
 5      266GB   316GB   50.0GB  logical   ext4
 6      316GB   324GB   7999MB  logical   linux-swap(v1)
 7      324GB   344GB   20.0GB  logical   ext4
 8      344GB   364GB   20.0GB  logical
 ```
 
In the following example, mkpartfs will create a new fat32 partition of size 127GB.
```
(parted) mkpartfs logical fat32 372737 500000
```

As you see below, the partition number 9 is successfully created.
```
(parted) print
Model: ATA WDC WD5000BPVT-7 (scsi)
Disk /dev/sda: 500GB
Sector size (logical/physical): 512B/4096B
Partition Table: msdos
Number  Start   End     Size    Type      Filesystem Flags
 1      1049kB  106MB   105MB   primary   fat16      diag
 2      106MB   15.8GB  15.7GB  primary   ntfs       boot
 3      15.8GB  266GB   251GB   primary   ntfs
 4      266GB   500GB   234GB   extended
 5      266GB   316GB   50.0GB  logical   ext4
 6      316GB   324GB   7999MB  logical   linux-swap(v1)
 7      324GB   344GB   20.0GB  logical   ext4
 8      344GB   364GB   20.0GB  logical
 9      373GB   500GB   127GB   logical   fat32      lba
(parted)
```

Resize Partition from One Size to Another Using resize
Using resize parted command, you can increase or decrease the partition size of a partition as shown in the example below.

```
(parted) resize 9
Start?  [373GB]? 373GB
End?  [500GB]? 450GB
```

As shown above, parted command will always warn whenever you are attempting to do something dangerous (i.e : rm, resize, mkfs).

The size of partition 9 is actually reduced from 127GB to 77GB. Verify that the partition is resized properly using the print command as shown below.
```
(parted) print
Model: ATA WDC WD5000BPVT-7 (scsi)
Disk /dev/sda: 500GB
Sector size (logical/physical): 512B/4096B
Partition Table: msdos
Number  Start   End     Size    Type      Filesystem Flags
 1      1049kB  106MB   105MB   primary   fat16      diag
 2      106MB   15.8GB  15.7GB  primary   ntfs       boot
 3      15.8GB  266GB   251GB   primary   ntfs
 4      266GB   500GB   234GB   extended
 5      266GB   316GB   50.0GB  logical   ext4
 6      316GB   324GB   7999MB  logical   linux-swap(v1)
 7      324GB   344GB   20.0GB  logical   ext4
 8      344GB   364GB   20.0GB  logical
 9      373GB   450GB   77.3GB  logical   fat32      lba
 ```
Parted allows you to type unambiguous abbreviation for commands like “p” for print, “sel” for select,etc.

Copy Data from One Partition to Another Using cp

The entire data from one partition can be copied to another partition using the cp command. You should also remember that the content of the destination will be deleted before copy starts. Make sure that the destination partition has enough size to hold the data from the source partition.

Using the “p” command (print) to display the current partition layout.

```
(parted) p
Model: ATA WDC WD5000BPVT-7 (scsi)
Disk /dev/sda: 500GB
Sector size (logical/physical): 512B/4096B
Partition Table: msdos
Number  Start   End     Size    Type      Filesystem Flags
 1      1049kB  106MB   105MB   primary   fat16      diag
 2      106MB   15.8GB  15.7GB  primary   ntfs       boot
 3      15.8GB  266GB   251GB   primary   ntfs
 4      266GB   500GB   234GB   extended
 5      266GB   316GB   50.0GB  logical   ext4
 6      316GB   324GB   7999MB  logical   linux-swap(v1)
 7      324GB   344GB   20.0GB  logical   ext4
 8      344GB   364GB   20.0GB  logical   ext2
 9      373GB   450GB   77.3GB  logical   fat32      lba
10      461GB   500GB   39.2GB  logical   ext2
```

It is recommended to unmount both source and destination partition before doing copy. In this example we are going to copy the content from partition 8 to partition 10.

The following shows the content of the corresponding partitions before copy.

```
# mount /dev/sda8 /mnt
# cd /mnt
# ls -l
total 52
-rw-r--r-- 1 root root
-rw-r--r-- 1 root root
# umount /mnt
# mount /dev/sda10 /mnt
# cd /mnt
# ls -l
total 48
 0 2011-09-26 22:52 part8
20 2011-09-26 22:52 test.txt
-rw-r--r-- 1 root root 0 2011-09-26 22:52 part10
```

Use the parted cp command to copy partition 8 to partition 10 as shown below.
```
(parted) cp 8 10
growing file system... 95%      (time left 00:38)
```

The following shows the content of the partition 10 after the copy. As you see below, the content of partition 8 is copied over (overwritten) to the partition 10.
```
# mount /dev/sda10 /mnt
# cd /mnt
# ls -l
total 52
-rw-r--r-- 1 root root
-rw-r--r-- 1 root root
 0 2011-09-26 22:52 part8
20 2011-09-26 22:52 test.txt
```
Note: When you copy across partitions of different filesystem(for example src : ext2 and dst : ext4), the destination partition’s file system is actually converted to the file system of source partition (i.e : ext2) .

Remove Partition from a Selected Hard Disk Using rm
To delete an unwanted or unused partition, use the parted rm command and specify the partition number as shown below.
```
(parted) rm
Partition number? 9
(parted)
```