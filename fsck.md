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