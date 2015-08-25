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