# How do I know if my hadoop hdfs filesystem has corrupt blocks, and how do I fix it?

The easiest way to determine this is to run an fsck on the filesystem. If you have setup your hadoop environment variables you should be able to use a path of /, if not hdfs://ip.or.hostname:50070/.
```
hdfs fsck /
```
or.
```
hdfs fsck hdfs://ip.or.hostname:50070/
```

###If the end of your output looks something like this, you have corrupt blocks on your fs.

```
.............................Status: CORRUPT
 Total size: 3453345169348 B (Total open files size: 664 B)
 Total dirs: 15233
 Total files: 14029
 Total symlinks: 0 (Files currently being written: 8)
 Total blocks (validated): 40961 (avg. block size 84308126 B) (Total open file blocks (not validated): 8)
 ********************************
 CORRUPT FILES: 2
 MISSING BLOCKS: 2
 MISSING SIZE: 15731297 B
 CORRUPT BLOCKS: 2
 ********************************
 Corrupt blocks: 2
 Number of data-nodes: 12
 Number of racks: 2
FSCK ended at Fri Mar 27 XX:03:21 UTC 201X in XXX milliseconds

The filesystem under path '/' is CORRUPT
```