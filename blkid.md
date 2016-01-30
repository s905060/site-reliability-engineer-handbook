# blkid

```
sudo blkid -o list
```

lists all devices with labels:

```
device          fs_type  label     mount point         UUID
----------------------------------------------------------------------------------
/dev/sda1       ntfs     WINRE_DRV (not mounted)       604C3A6A4C3A3B5C
/dev/sda2       vfat     SYSTEM_DRV (not mounted)      6C3C-72E3
/dev/sda3       vfat     LRS_ESP   (not mounted)       5240-1BEE
/dev/sda5       ntfs     Windows8_OS /media/Win8       A47A42FF7A42CDAC
/dev/sda6       ntfs     Daten     /media/Daten        72860971860936DF
```