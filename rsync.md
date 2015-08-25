# RSYNC

Important features of rsync
* Speed: First time, rsync replicates the whole content between the source and destination directories. Next time, rsync transfers only the changed blocks or bytes to the destination location, which makes the transfer really fast.
* Security: rsync allows encryption of data using ssh protocol during transfer.
* Less Bandwidth: rsync uses compression and decompression of data block by block at the sending and receiving end respectively. So the bandwidth used by rsync will be always less compared to other file transfer protocols.
* Privileges: No special privileges are required to install and execute rsync
