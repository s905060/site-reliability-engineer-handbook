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