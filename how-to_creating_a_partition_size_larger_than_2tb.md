# How-To Creating a Partition Size Larger Than 2TB

Linux GPT Kernel Support

EFI GUID Partition support works on both 32bit and 64bit platforms. You must include GPT support in kernel in order to use GPT. If you don't include GPT support in Linux kernelt, after rebooting the server, the file system will no longer be mountable or the GPT table will get corrupted. By default Redhat Enterprise Linux / CentOS comes with GPT kernel support. However, if you are using Debian or Ubuntu Linux, you need to recompile the kernel. Set CONFIG_EFI_PARTITION to y to compile this feature.
```
File Systems
Partition Types
[*] Advanced partition selection
[*] EFI GUID Partition support (NEW)
```

Find Out Current Disk Size

Type the following command:
```
# fdisk -l /dev/sdb
```

Sample outputs:
```
Disk /dev/sdb: 3000.6 GB, 3000592982016 bytes
255 heads, 63 sectors/track, 364801 cylinders
Units = cylinders of 16065 * 512 = 8225280 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x00000000
Disk /dev/sdb doesn't contain a valid partition table
```

Linux Create 3TB partition size

To create a partition start GNU parted as follows:
```
# parted /dev/sdb
```
Output:
```
GNU Parted 2.3
Using /dev/sdb
Welcome to GNU Parted! Type 'help' to view a list of commands.
(parted)
```

Creates a new GPT disklabel i.e. partition table:
```
(parted) mklabel gpt
```
Sample outputs:
```
Warning: The existing disk label on /dev/sdb will be destroyed and all data on this disk will be lost. Do you want to continue?
Yes/No? yes
(parted)
```
Next, set the default unit to TB, enter:
```
(parted) unit TB
```

To create a 3TB partition size, enter:
```
(parted) mkpart primary 0 0
```
OR
```
(parted) mkpart primary 0.00TB 3.00TB
```
To print the current partitions, enter:
```
(parted) print
```
Sample outputs:
```
Model: ATA ST33000651AS (scsi)
Disk /dev/sdb: 3.00TB
Sector size (logical/physical): 512B/512B
Partition Table: gpt
Number  Start   End     Size    File system  Name     Flags
 1      0.00TB  3.00TB  3.00TB  ext4         primary
```
Quit and save the changes, enter:
```
(parted) quit
```
Sample outputs:
```
Information: You may need to update /etc/fstab.
```

Use the mkfs.ext3 or mkfs.ext4 command to format the file system, enter:
```
# mkfs.ext3 /dev/sdb1
```
OR
```
# mkfs.ext4 /dev/sdb1
```
Sample outputs:
```
mkfs.ext4 /dev/sdb1
mke2fs 1.41.12 (17-May-2010)
Filesystem label=
OS type: Linux
Block size=4096 (log=2)
Fragment size=4096 (log=2)
Stride=0 blocks, Stripe width=0 blocks
183148544 inodes, 732566272 blocks
36628313 blocks (5.00%) reserved for the super user
First data block=0
Maximum filesystem blocks=4294967296
22357 block groups
32768 blocks per group, 32768 fragments per group
8192 inodes per group
Superblock backups stored on blocks:
	32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208,
	4096000, 7962624, 11239424, 20480000, 23887872, 71663616, 78675968,
	102400000, 214990848, 512000000, 550731776, 644972544
Writing inode tables: done
Creating journal (32768 blocks): done
Writing superblocks and filesystem accounting information: done
This filesystem will be automatically checked every 31 mounts or
180 days, whichever comes first.  Use tune2fs -c or -i to override.
```

Type the following commands to mount /dev/sdb1, enter:
```
# mkdir /data
# mount /dev/sdb1 /data
# df -H
```

Sample outputs:
```
Filesystem             Size   Used  Avail Use% Mounted on
/dev/sdc1               16G   819M    14G   6% /
tmpfs                  1.6G      0   1.6G   0% /lib/init/rw
udev                   1.6G   123k   1.6G   1% /dev
tmpfs                  1.6G      0   1.6G   0% /dev/shm
/dev/sdb1              3.0T   211M   2.9T   1% /data
```

Make sure you replace /dev/sdb1 with actual RAID or Disk name or Block Ethernet device such as /dev/etherd/e0.0. Do not forget to update /etc/fstab, if necessary. Also note that booting from a GPT volume requires support in your BIOS / firmware. This is not supported on non-EFI platforms. I suggest you boot server from another disk such as IDE / SATA / SSD disk and store data on /data.