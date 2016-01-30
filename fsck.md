# FSCK

fsck stands for "file system consistency check". On most systems, fsck is run at boot time if certain conditions are detected. Usually, these conditions are:

* A file system is marked as "dirty" — its written state is inconsistent with data that was scheduled to be written; or,
* A file system has been mounted a set number of times without being checked.

The fsck command itself interacts with a matching filesystem-specific fsck command created by the filesystem's authors. Regardless of filesystem type, fsck generally has three modes of operation:
* Check for errors, and prompt the user interactively to decide how to resolve individual problems;
* Check for errors, and attempt to fix any errors automatically; or,
* Check for errors, and make no attempt to repair them, but display the errors on standard output.

While normally run at boot time, fsck can be run manually on unmounted filesystems by a superuser.

`fsck` is used to check, and optionally repair, one or more filesystems.
filesys can be a device name (e.g. `/dev/hdc1`, `/dev/sdb2`), a mount point (e.g. `/`, `/usr`, `/home`), or an `ext2` label or UUID specifier (for example "`UUID=8868abf6-88c5-4a83-98b8-bfc24057f7bd`" or "`LABEL=root`").

Normally, fsck will try to handle filesystems on different physical disk drives in parallel to reduce the total amount of time needed to check all of them.

If no filesystems are specified on the command line, and the `-A` option is not specified, `fsck` will default to checking filesystems in `/etc/fstab` serially. This is equivalent to the combination of the `-A` and `-s` options.

The exit code returned by `fsck` is a unique number representing the sum of the following condition values:

* **0** - No errors
* **1** - Filesystem errors corrected
* **2** - System should be rebooted
* **4** - Filesystem errors left uncorrected
* **8** - Operational error
* **16** - Usage or syntax error
* **32** - Fsck canceled by user request
* **128** - Shared-library error

The exit code returned when multiple filesystems are checked is the bitwise OR of the exit codes for each filesystem that is checked.

In actuality, fsck is simply a front-end for the various filesystem checkers (`fsck.fstype`) available under Linux. The filesystem-specific checker is searched for in `/sbin` first, then in` /etc/fs` and `/etc`, and finally in the directories listed in the `PATH` environment variable.

Read filesystem-specific checker manual pages for further details. For example, to learn more about ext3-specific `fsck` checking, run:
`man fsck.ext3`

Dropping Into Single-User Mode
These instructions should help you bring your Linux system into single-user mode and unmount any filesystems you'd like to check with fsck.
First, initiate runlevel 1 (single-user mode) with the init command:
```
sudo init 1
```

Now unmount the filesystem using umount. For instance, if /home is mounted on /dev/sda2, you could run:

```
umount /home
```

...or:
```
umount /dev/sda2
```

Make sure to run umount for any filesystem you want to check with fsck.

Checking Filesystems
```
fsck /dev/sda2
```
This command will attempt to check /dev/sda2, and report any errors it finds.
```
fsck -y /dev/sda2
```

Check /dev/sda2, and attempt to automatically fix any errors found.
```
fsck -A
```

Check all configured filesystems. fsck will process the file /etc/fstab and check all file systems listed there. Systems will be checked in order of their <pass> value, as listed in the fstab file. Systems with a pass value of 0 will be skipped; the system with a pass value of 1 will be listed first, and remaining systems will be checked in ascending order of their pass value.

```
cat /etc/fstab
```

View all configured filesystems. Output will resemble the following:
```
# /etc/fstab: static file system information.
#
# Use 'blkid' to print the universally unique identifier for a
# device; this may be used with UUID= as a more robust way to name devices
# that works even if disks are added and removed. See fstab man page.
#
# <file system> <mount point>   <type>  <options>       <dump>  <pass>
# / was on /dev/sda5 during installation
UUID=c3a6839b-00f1-4cf4-8b00-e61fbcdba6c0 /               ext4    errors=remount-ro 0       1
# /home was on /dev/sda7 during installation
UUID=afceabb6-a3f4-41c2-9ae6-0393d85c7c70 /home           ext4    defaults        0       2
# swap was on /dev/sda6 during installation
UUID=c6ca8b8f-0b46-4c06-a934-a9dd3525faa7 none            swap    sw              0       0
#/dev/sdb1       /media/usb0     auto    rw,user,noauto  0       0
```
```
ls /sbin/fsck.*
```

View all filesystems that can be checked with fsck. Filesystems will appear as extensions to the fsck.* files; for example:
```
fsck.cramfs  fsck.ext3  fsck.ext4dev  fsck.minix  fsck.nfs      fsck.reiserfs  fsck.xfs
fsck.ext2    fsck.ext4  fsck.jfs      fsck.msdos  fsck.reiser4  fsck.vfat
```

```
fsck -n /dev/sda2
```

Check `/dev/sda2` for errors, but do not attempt to repair them; instead, print any errors to standard output.
```
fsck -f /dev/sda2
```

Normally, fsck will skip parts of the filesystem marked as "clean" — meaning all pending writes were successfully made. The `-f ("force")` option specifies that `fsck` should check parts of the filesystem even if they are not "dirty". The result is a less efficient, but a more thorough check.
```
fsck -t ext2 /dev/fd0
```

This command will check the ext2 filesystem on the floppy diskette device `/dev/fd0`.
