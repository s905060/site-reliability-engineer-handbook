# How to solve fsck.ext4: Unable to resolve UUID/LABEL



## Symptoms

After replacing the disk, hardware node won't boot normally. Following error is displayed on the screen:
```bash
fsck.ext4: Unable to resolve 'UUID=xxxxxxxx-xxx-xxx-xxx-xxxxxxxxxxxx' [FAILED]

Give root password for maintenance
(or type  Control-D to continue):
```
## Cause

After removing the disk, corresponding entry in `/etc/fstab` still exists. During the booting process, system tries to locate the device using the UUID specified in `/etc/fstab`, but fails since the disk was removed.

## Resolution

1. Type root password, the system will boot with read-only mounted partitions.

2. Remount the root partition to be able to modify its content:
```bash
~# mount -o remount,rw /
```
3. Comment out entries in `/etc/fstab` for the removed devices.

4. Reboot the node.



