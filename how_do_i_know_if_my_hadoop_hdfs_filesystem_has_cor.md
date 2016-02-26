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

How do I know which files have blocks that are corrupt?
The output of the fsck above will be very verbose, but it will mention which blocks are corrupt. We can do some grepping of the fsck above so that we aren't "reading through a firehose".
```
hdfs fsck / | egrep -v '^\.+$' | grep -v replica | grep -v Replica
```
or
```
hdfs fsck hdfs://ip.or.host:50070/ | egrep -v '^\.+$' | grep -v replica | grep -v Replica
```

This will list the affected files, and the output will not be a bunch of dots, and also files that might currently have under-replicated blocks (which isn't necessarily an issue). The output should include something like this with all your affected files.
```
/path/to/filename.fileextension: CORRUPT blockpool BP-1016133662-10.29.100.41-1415825958975 block blk_1073904305

/path/to/filename.fileextension: MISSING 1 blocks of total size 15620361 B
```

The next step would be to determine the importance of the file, can it just be removed and copied back into place, or is there sensitive data that needs to be regenerated?

If it's easy enough just to replace the file, that's the route I would take.

Remove the corrupted file from your hadoop cluster

This command will move the corrupted file to the trash.
```
hdfs dfs -rm /path/to/filename.fileextension
hdfs dfs -rm hdfs://ip.or.hostname.of.namenode:50070/path/to/filename.fileextension
```

Or you can skip the trash to permanently delete (which is probably what you want to do)
```
hdfs dfs -rm -skipTrash /path/to/filename.fileextension
hdfs dfs -rm -skipTrash hdfs://ip.or.hostname.of.namenode:50070/path/to/filename.fileextension
```