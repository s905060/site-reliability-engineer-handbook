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

## 4.udevadm
yet again retrieve the same information using udevadm command:
```bash
#udevadm info -q all -n /dev/sda2 | grep uuid
S: disk/by-uuid/333db32c-b91e-41da-86c7-801c88059660
```

## 5.udevinfo
Same as previous command now we use udevinfo which actually 'udevadm info':
```bash
#udevinfo -q all -n /dev/sda2 | grep uuid
S: disk/by-uuid/333db32c-b91e-41da-86c7-801c88059660
```

## 6.vol_id
As it was not already enough we can also use a vol_id command to retrieve UUID from partition:
```bash
# vol_id /dev/sda2 | grep UUID
ID_FS_UUID=333db32c-b91e-41da-86c7-801c88059660
ID_FS_UUID_ENC=333db32c-b91e-41da-86c7-801c88059660
```

## 7.hwinfo
another way on how to access a UUID and the hardrives/block devices is to use hwinfo command. hwinfo is not installed by defualt so you may need to install it first:
```bash
hwinfo --block
```

## 8.Change UUID
Now let's talk about how to change a partition's UUID. First we need to install uuid command ( if not already installed ) which will help us to generate uuid string: EXAMPLE:
```bash
# uuid
3fa4ae0a-365b-11df-9470-000c29e84ddd
# uuid
46a967c2-365b-11df-ae47-000c29e84ddd
```

NOTE: on some systems you can have uuidgen command instead of uuid !

let's see how it works: old partition UUID:
```bash
# vol_id  /dev/hdb1
ID_FS_USAGE=filesystem
ID_FS_TYPE=ext3
ID_FS_VERSION=1.0
ID_FS_UUID=50722b6b-dfd8-4faf-bb27-220fd69b0deb
ID_FS_UUID_ENC=50722b6b-dfd8-4faf-bb27-220fd69b0deb
ID_FS_LABEL=
ID_FS_LABEL_ENC=
ID_FS_LABEL_SAFE=
```

now we use a tune2fs linux command to change /dev/hdb1 partition's UUID:
```bash
# tune2fs /dev/hdb1 -U `uuid`
tune2fs 1.41.3 (12-Oct-2008)
```

confirm changes:
```bash
#vol_id  /dev/hdb1
ID_FS_USAGE=filesystem
ID_FS_TYPE=ext3
ID_FS_VERSION=1.0
ID_FS_UUID=94d5ceae-365b-11df-81ee-000c29e84ddd
ID_FS_UUID_ENC=94d5ceae-365b-11df-81ee-000c29e84ddd
ID_FS_LABEL=
ID_FS_LABEL_ENC=
ID_FS_LABEL_SAFE=
```