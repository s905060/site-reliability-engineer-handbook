# How to retrieve and change partition's UUID Universally Unique Identifier on linux

On some unix systems a hard drives partition is referred by to as Universally Unique Identifier( UUID ) instead of the location of the it relevant block device file for example like in /etc/fstab:

```bash
UUID=A00CB51F0CB4F180      /my/moutn/point     vfat    
defaults,umask=007,gid=46         0       0
```

In this case it would be harder to find which partition is mounted behind /my/moutn/point. Here are some ways how to retrieve relevant partition UUID. I have listed here 7 ways on how to retrieve UUID so let me know if you know more.


## 1.blkid

```
# blkid
/dev/sda1: UUID="A00CB51F0CB4F180" TYPE="ntfs"
/dev/sda2: UUID="333db32c-b91e-41da-86c7-801c88059660" 
TYPE="ext3"
/dev/hdb1: UUID="ec4dfd3a-5811-4967-a28d-ba76c8ad55a9" 
TYPE="ext3"
/dev/hdb5: TYPE="swap"
```

or you can specify argument to retrieve a single partition UUID:

```
# blkid /dev/sda2
/dev/sda2: UUID="333db32c-b91e-41da-86c7-801c88059660" 
TYPE="ext3"
```

## 2./dev/disk/by-uuid/
another way is to consult /dev/disk/by-uuid/ :

```bash
ls -l /dev/disk/by-uuid/
total 0
lrwxrwxrwx 1 root root 10 2010-03-23 23:56 333db32c-b91e-41da-86c7-801c88059660 -> ../../sda2
lrwxrwxrwx 1 root root 10 2010-03-23 23:56 A00CB51F0CB4F180 -> ../../sda1
lrwxrwxrwx 1 root root 10 2010-03-23 23:56 ec4dfd3a-5811-4967-a28d-ba76c8ad55a9 -> ../../hdb1
```

## 3./dev/.udev/db/
the partition information is also stored by udev. cat the file which ends as your block device file of your partition. For example lets get uuid for sda2:
```
# cat /dev/.udev/db/*sda2 | grep uuid
S:disk/by-uuid/333db32c-b91e-41da-86c7-801c88059660
```

in other words this is the same UUID which we retrieved previously.