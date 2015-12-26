# Understanding hadoop fsck, fs -dus, -count -q size output

Many hadoop users often confuse size and significance hadoop fsck, hadoop fs -dus, hadoop -count -q and other hadoop file system command output. 
Here to do a summary of these issues. First we have to clear two concepts:

* Logical space, a distributed file system that is on the real file size
* Physical space, a distributed file system that is present on the actual space occupied by the file

###Why logical space is generally not equal to the physical space? 

In order to ensure the reliability of distributed file system files, often to save multiple backups (usually 3 parts), under long backups is not 1, the general logic of the physical space would be several times the space. Relationship is as follows:

>HDFS physical space = logical space * block backups

`hadoop fsck and hadoop fs -dus`

execution hadoop fsck and hadoop fs -dus display file size represents the logical files take up space.

```
$ Hadoop fsck / path / to / Directory
  Total size :     16,565,944,775,310 B     <===  look here 
 Total dirs :     3922 
 Total Files :    418 464 
 Total Blocks ( validated ):       502705  ( avg . Block size 32,953,610 B ) 
 Minimally replicated Blocks :    502 705  ( 100.0  %) 
 Over - replicated Blocks :         0  ( 0.0  %) 
 Under - replicated Blocks :        0  ( 0.0  %) 
 Mis - replicated Blocks :          0  ( 0.0  %) 
 Default replication factor :     3 
 Average Block replication :      3.0 
 Corrupt Blocks :                 0 
 Missing Replicas :               0  ( 0.0  %) 
 Number of Data - nodes :           18 
 Number of racks :                1 
FSCK ended at Thu  Oct  20  20 : 49 : 59 CET 2011  in  7516 milliseconds
 
The filesystem path under '/ path / to / Directory'  is HEALTHY

$ Hadoop FS - DUS / path / to / Directory
hdfs : // master: 54310 / path / to / Directory 16,565,944,775,310 <=== see here
```

As examples of commands seen, hadoop fsck and hadoop fs -dus report file sizes are actually occupied HDFS file size, that this space is not backed up several blocks counted. Real physical space occupied by the file space = logic block backup data, namely 16565944775310 3 = 49697834325930, 49697834325930 This is a physical space.

`hadoop fs -count -q`
by performing hadoop fs -count -q / path / to / directory can see this directory real space usage. Execution results are as follows:

```
$ Hadoop FS - count - q / path / to / Directory
  QUOTA REMAINING_QUOTA SPACE_QUOTA REMAINING_SPACE_QUOTA DIR_COUNT FILE_COUNT CONTENT_SIZE FILE_NAME
   none INF   54,975,581,388,800           5277747062870         3922        418,464     16565944775310 hdfs : // master: 54310 / path / to / Directory
```

fs -count -q outputs 8, respectively, as follows:

|Quota namespace (limited number of files)	|The remaining quota namespace	|Quota physical space (limited footprint size)	|The remaining physical space	|Catalog Number statistics	|File count statistics	|The total size of the directory logical space	path|
|--|--|--|--|--|--|--|
|--|--|--|--|--|--|--|

As can be seen in more detail you can see a directory space and qutoa occupancy by hadoop fs -count -q, it contains the physical space, logical space, number of files, directory number, qutoa remaining amount.