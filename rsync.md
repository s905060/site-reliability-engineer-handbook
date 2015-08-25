# RSYNC

Important features of rsync
* Speed: First time, rsync replicates the whole content between the source and destination directories. Next time, rsync transfers only the changed blocks or bytes to the destination location, which makes the transfer really fast.
* Security: rsync allows encryption of data using ssh protocol during transfer.
* Less Bandwidth: rsync uses compression and decompression of data block by block at the sending and receiving end respectively. So the bandwidth used by rsync will be always less compared to other file transfer protocols.
* Privileges: No special privileges are required to install and execute rsync

Synchronize Two Directories in a Local Server
To sync two directories in a local computer, use the following rsync -zvr command.
```
$ rsync -zvr /var/opt/installation/inventory/ /root/temp
building file list ... done
sva.xml
svB.xml
.
sent 26385 bytes  received 1098 bytes  54966.00 bytes/sec
total size is 44867  speedup is 1.63
```
* -z is to enable compression
* -v verbose
* -r indicates recursive

Now let us see the timestamp on one of the files that was copied from source to destination. As you see below, rsync didn’t preserve timestamps during sync.
```
$ ls -l /var/opt/installation/inventory/sva.xml
/root/temp/sva.xml
-r--r--r-- 1 bin  bin  949 Jun 18  2009
/var/opt/installation/inventory/sva.xml
-r--r--r-- 1 root bin  949 Sep  2  2009 /root/temp/sva.xml
```

Preserve timestamps during Sync using rsync -a
rsync option -a indicates archive mode. -a option does the following,
* Recursive mode
* Preserves symbolic links
* Preserves permissions
* Preserves timestamp
* Preserves owner and group

Now, executing the same command provided in example 1 (But with the rsync option -a) as shown below:

```
$ rsync -azv /var/opt/installation/inventory/ /root/temp/
building file list ... done
./
sva.xml
svB.xml
.
sent 26499 bytes  received 1104 bytes  55206.00 bytes/sec
total size is 44867  speedup is 1.63
```

As you see below, rsync preserved timestamps during sync.
```
$ ls -l /var/opt/installation/inventory/sva.xml
/root/temp/sva.xml
-r--r--r-- 1 root  bin  949 Jun 18  2009
/var/opt/installation/inventory/sva.xml
-r--r--r-- 1 root  bin  949 Jun 18  2009
/root/temp/sva.xml
```

Synchronize Only One File

To copy only one file, specify the file name to rsync command, as shown below.
```
$ rsync -v /var/lib/rpm/Pubkeys /root/temp/
Pubkeys
sent 42 bytes  received 12380 bytes  3549.14 bytes/sec
total size is 12288  speedup is 0.99
```

Synchronize Files From Local to Remote

rsync allows you to synchronize files/directories between the local and remote system.

```
$ rsync -avz /root/temp/
thegeekstuff@192.168.200.10:/home/thegeekstuff/temp/
Password:
building file list ... done
./
rpm/
rpm/Basenames
rpm/Conflictname
sent 15810261 bytes  received 412 bytes  2432411.23
bytes/sec
total size is 45305958 speedup is 2.87
```

Synchronize Files From Remote to Local
When you want to synchronize files from remote to local, specify remote path in source and local path in target as shown below.
```
$ rsync -avz thegeekstuff@192.168.200.10:/var/lib/rpm
/root/temp
Password:
receiving file list ... done
rpm/
rpm/Basenames
.
sent 406 bytes  received 15810230 bytes  2432405.54
bytes/sec
total size is 45305958  speedup is 2.87
```