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