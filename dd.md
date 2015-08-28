# DD

Examples

Caution: Use dd cautiously — improper usage or entering the wrong values could inadvertently wipe, destroy, or overwrite the data on your hard drive.
```
dd if=/dev/sr0 of=/home/hope/exampleCD.iso bs=2048 conv=noerror,sync
```

Create a ISO disc image from the CD in the computer.
```
dd if=/dev/sda of=~/disk1.img
```

Create an img file of the /dev/sda hard drive. To restore that image type: **dd if=disk1.img of=/dev/sda**
```
dd if=/dev/sda of=/dev/sdb
```

Copy the contents from the if= drive /dev/sda to the of= drive /dev/sdb.