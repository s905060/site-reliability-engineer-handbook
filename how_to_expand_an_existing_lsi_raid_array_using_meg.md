# How to expand an existing LSI raid array using MegaCli

The exact commands to do this vary on your current configuration and number of disks in the raid. Before adding in the disks you need to get a feel for your current setup by using the following commands.

You can see all the currently attached disks with the following command. Each disk will have an ‘Enclosure Device ID’ and a ‘Slot Number’ – these are how you reference the disks when using MegaCli.

```bash
$ MegaCli64 -PDList -aALL
```

You can see the currently configured raid arrays with the following command. The main points are the raids data size, number of disks, and raid level.

```bash
$ MegaCli64 -LDInfo -Lall -aALL
```

The main conventions that LSI raid cards use are the ideas of enclosures and slots for disks:
```bash
Enclosure Device ID: 252
Slot Number: 3
```
— So this fictional disk would be referenced by [252:3]
Also for the raid arrays there is a `logical` and `adapter` value. If you only have one raid card and one raid setup on the system both your adapter and logical are going to be 0.

For this example we will say our system already has three drives installed and we are going to install one more. In this case the output of `MegaCli64 -PDList -aALL` would have shown three drives and the last one would have an EID of 252 (this varies for each raid card installation) and a Slot Number of 2 (device numbering starts at 0). Now that we know our current slot numbers we can physically install the new drive. After installation the last Slot Number shown from `MegaCli64 -PDList -aALL`  will be 3 since there are now four drives attached to the raid card.

The command to expand the raid with the new drive will be:
```bash
$ MegaCli64 -LDRecon -Start -r5 -Add -PhysDrv[252:3] -l0 -a0
```

– This would add one drive [252:3] to logical 0 on adapter 0 and it is a Raid level 5

If you were adding multiple drives the Enclosure IDs and Slot Numbers just become a comma separated list like so:
```bash
$ MegaCli64 -LDRecon -Start -r5 -Add -PhysDrv[252:3,252:4,252:5,252:6] -l0 -a0
```

To check on the progress of the expansion and status of the device run the following commands:

```bash
$ MegaCli64 -LDRecon -ShowProg -l0 -a0
$ MegaCli64 -LDInfo -l0 -a0
```

It can take a day or more to finish reconstruction depending on the size of the raid. In this example we added a single 500GB drive to an raid5 array of three 500GB drives – it took approximately 32 hours to finish reconstruction.


## Resizing the partition

Note: The following does not apply to LVM’s

Once your raid array has finished reconfiguring with the new disks you will still need to resize both your partition and the filesystem.

First you will need to make sure the partition is not mounted. In our example it is mounted to /data and the device is /dev/sdb
```bash
$ umount /data
```

Check to make sure it’s not listed in the current mounts anymore:
```bash
$ mount
```

Next you will resize the partition by looking at the current partitions start point, remove the partition, and recreate the partition with a larger end size:
```bash
[root@raid /]# parted /dev/sdb
GNU Parted 2.1
Using /dev/sdb
Welcome to GNU Parted! Type ‘help’ to view a list of commands.
(parted) p
Model: LSI MR9261-8i (scsi)
Disk /dev/sdc: 998GB
Sector size (logical/physical): 512B/512B
Partition Table: gpt

Number     Start          End          Size     File system Name Flags
1                1049kB     200GB    200GB         xfs

(parted) rm 1
(parted) mkpart
Partition name? []?
File system type? [ext2]? xfs
Start? 1049kB
End? -1
(parted) p
Model: LSI MR9261-8i (scsi)
Disk /dev/sdb: 998GB
Sector size (logical/physical): 512B/512B
Partition Table: gpt

Number    Start       End        Size      File system Name Flags
1               1049kB   998GB   998GB          xfs

(parted) q
Information: You may need to update /etc/fstab.

[root@raid /]#
```

And finally remount /dev/sdb1 back to /data:
```bash
$ mount /dev/sdb1 /data
```


## Resizing the filesystem

Next we need to resize the filesystem on /dev/sdb1. The method used depends on which filesystem type you are using on that partition. If you are not sure you can see the filesystem used on every partition that is currently mounted with the command `fsck -N`.

To automatically resize an ext4 filesystem to the partitions full size:
```bash
$ resize2fs /data
```

To automatically resize an xfs filesystem to the partitions full size:
```bash
$ xfs_growfs /data
```

You should be able to see the new size for /data with:
```bash
$ df -h
```