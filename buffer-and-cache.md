## Buffer and Cache

Buffers are associated with a specific block device, and cover caching of filesystem metadata as well as tracking in-flight pages. The cache only contains parked file data. That is, the buffers remember what's in directories, what file permissions are, and keep track of what memory is being written from or read to for a particular block device. The cache only contains the contents of the files themselves.

**Cache Pages:**

A cache is the part of the memory which transparently stores data so that future requests for that data can be served faster. This memory is utilized by the kernel to cache disk data and improve i/o performance.

The Linux kernel is built in such a way that it will use as much RAM as it can to cache information from your local and remote filesystems and disks. As the time passes over various reads and writes are performed on the system, kernel tries to keep data stored in the memory for the various processes which are running on the system or the data that of relevant processes which would be used in the near future. The cache is not reclaimed at the time when process get stop/exit, however when the other processes requires more memory then the free available memory, kernel will run heuristics to reclaim the memory by storing the cache data and allocating that memory to new process.

When any kind of file/data is requested then the kernel will look for a copy of the part of the file the user is acting on, and, if no such copy exists, it will allocate one new page of cache memory and fill it with the appropriate contents read out from the disk.

The data that is stored within a cache might be values that have been computed earlier or duplicates of original values that are stored elsewhere in the disk. When some data is requested, the cache is first checked to see whether it contains that data. The data can be retrieved more quickly from the cache than from its source origin.

SysV shared memory segments are also accounted as a cache, though they do not represent any data on the disks. One can check the size of the shared memory segments using ipcs -m command and checking the bytes column.

**Buffers :**

Buffers are the disk block representation of the data that is stored under the page caches. Buffers contains the metadata of the files/data which resides under the page cache. Example: When there is a request of any data which is present in the page cache, first the kernel checks the data in the buffers which contain the metadata which points to the actual files/data contained in the page caches. Once from the metadata the actual block address of the file is known, it is picked up by the kernel for processing.

