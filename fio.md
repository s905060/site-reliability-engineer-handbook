# fio



## **Fio Test Options and Examples**


## **blocksize**

This options determines the block size for the I/O units used during the test. The default value for blocksize is 4k (4KB). This option can be set for both read and write tests. For random workloads, the default value of 4k is typically used, for sequential workloads, a value of 1M (MB) is usually used. Change this value to whatever your production environment uses so that you are replicating the real world scenario as much as possible. If your servers deal with 4K block sizes 99% of the time, then why test out performance using 1MB blocksize?

```bash
--blocksize=4k (default)
``


## **ioengine**

By default, Fio will run tests using the sync io engine, but if you want to change the engine used, you can. There are many different options you could change this value to, but on Linux the most common options are sync or libaio if the kernel supports it.

```bash
--ioengine=sync (default)
```


## **iodepth**

The iodepth option defines the amount of IO units that will continue to hammer a file with requests during the test. If you are using the default sync ioengine, then increasing the iodepth beyond the default value of 1 will not have an effect. Even if you change the ioengine to use something like libaio the OS might restrict the maximum iodepth and ignore the specified value. Because of this I recommend starting off testing with an iodepth of 1 and raise this to something like 16 and test again, if you do not see any performance differences then you may not want to even specify this option, especially if you have set directio to a value of 1. Again, every server / OS is different so test out a few combinations of options before you start recording results.

```bash
--iodepth=1 (default)
```


## **direct**

This option tells Fio whether or not it should use direct IO, or buffered IO. The default value is "0" which means that Fio will use use buffered I/O for the test. If you set this value to 1 then Fio will avoid using buffered IO, usually this is similar to O_DIRECT. Using buffered IO will almost always provide better performance than non-buffered IO, especially for read tests, or if you are testing out a server with a very large amount of RAM, using non-buffered IO helps to avoid inflated results. If you every run a test and Fio tells you that an SSD performed 600,000 IOPs, odds are it's not, and Fio is reading out of RAM, which will obviously be faster.

```bash
--direct=0 (default)
```


## **fsync**

The fsync option tells Fio how often it should use fsync to flush "dirty data" to disk. By default this value is set to 0 which means "don't sync". Many applications perform like this and leave it up to Linux to figure out when to flush data from memory to disk. If your application or server always flushes every write to disk (meta-data and data) then you should include this option and set it to a 1. If your application does not flush data to disk after each write, or you aren't too worried about potential data loss, then leave this value alone. Setting fsync to 1 will completely avoid the buffering of writes, so if you want to see the "worst case" performance IO performance for a block device, set fsync to 1 and run a random write test. Results will be much lower than without fsync, but since every single write operation has to get flushed to disk, the disk will be stressed.

```bash
--fsync=0 (default)
```

```bash
#! /bin/bash -x


# random read/write, 8K block, 16 jobs
fio --name=/dev/sdb --ioengine=libaio --iodepth=128 -rw=randrw --bs=8k --direct=1 --size=1G --numjobs=12 --runtime=30 --group_reporting

# random read/write, 64K block, 16 jobs
fio --name=/dev/sdb --ioengine=libaio --iodepth=128 -rw=randrw --bs=64k --direct=1 --size=1G --numjobs=12 --runtime=30 --group_reporting

# random read/write, 128K block, 16 jobs
fio --name=/dev/sdb --ioengine=libaio --iodepth=128 -rw=randrw --bs=128k --direct=1 --size=1G --numjobs=12 --runtime=30 --group_reporting

# random read/write, 256K block, 16 jobs
fio --name=/dev/sdb --ioengine=libaio --iodepth=128 -rw=randrw --bs=256k --direct=1 --size=1G --numjobs=12 --runtime=30 --group_reporting


# random write, 8K block, 16 jobs
fio --name=/dev/sdb --ioengine=libaio --iodepth=128 -rw=randwrite --bs=8k --direct=1 --size=1G --numjobs=12 --runtime=30 --group_reporting

# random write, 64K block, 16 jobs
fio --name=/dev/sdb --ioengine=libaio --iodepth=128 -rw=randwrite --bs=64k --direct=1 --size=1G --numjobs=12 --runtime=30 --group_reporting

# random write, 128K block, 16 jobs
fio --name=/dev/sdb --ioengine=libaio --iodepth=128 -rw=randwrite --bs=128k --direct=1 --size=1G --numjobs=12 --runtime=30 --group_reporting

# random write, 256K block, 16 jobs
fio --name=/dev/sdb --ioengine=libaio --iodepth=128 -rw=randwrite --bs=256k --direct=1 --size=1G --numjobs=12 --runtime=30 --group_reporting


# random write, 8K block, 16 jobs
fio --name=/dev/sdb --ioengine=libaio --iodepth=128 -rw=randread --bs=8k --direct=1 --size=1G --numjobs=12 --runtime=30 --group_reporting

# random write, 64K block, 16 jobs
fio --name=/dev/sdb --ioengine=libaio --iodepth=128 -rw=randread --bs=64k --direct=1 --size=1G --numjobs=12 --runtime=30 --group_reporting

# random write, 128K block, 16 jobs
fio --name=/dev/sdb --ioengine=libaio --iodepth=128 -rw=randread --bs=128k --direct=1 --size=1G --numjobs=12 --runtime=30 --group_reporting

# random write, 256K block, 16 jobs
fio --name=/dev/sdb --ioengine=libaio --iodepth=128 -rw=randread --bs=256k --direct=1 --size=1G --numjobs=12 --runtime=30 --group_reporting


# sequential write, 8K block, 16 jobs
fio --name=/dev/sdb --ioengine=libaio --iodepth=128 -rw=write --bs=8k --direct=1 --size=1G --numjobs=12 --runtime=30 --group_reporting

# sequential write, 64K block, 16 jobs
fio --name=/dev/sdb --ioengine=libaio --iodepth=128 -rw=write --bs=64k --direct=1 --size=1G --numjobs=12 --runtime=30 --group_reporting

# sequential write, 128K block, 16 jobs
fio --name=/dev/sdb --ioengine=libaio --iodepth=128 -rw=write --bs=128k --direct=1 --size=1G --numjobs=12 --runtime=30 --group_reporting

# sequential write, 256K block, 16 jobs
fio --name=/dev/sdb --ioengine=libaio --iodepth=128 -rw=write --bs=256k --direct=1 --size=1G --numjobs=12 --runtime=30 --group_reporting


# sequential read, 8K block, 16 jobs
fio --name=/dev/sdb --ioengine=libaio --iodepth=128 -rw=read --bs=8k --direct=1 --size=1G --numjobs=12 --runtime=30 --group_reporting

# sequential read, 64K block, 16 jobs
fio --name=/dev/sdb --ioengine=libaio --iodepth=128 -rw=read --bs=64k --direct=1 --size=1G --numjobs=12 --runtime=30 --group_reporting

# sequential read, 128K block, 16 jobs
fio --name=/dev/sdb --ioengine=libaio --iodepth=128 -rw=read --bs=128k --direct=1 --size=1G --numjobs=12 --runtime=30 --group_reporting

# sequential read, 256K block, 16 jobs
fio --name=/dev/sdb --ioengine=libaio --iodepth=128 -rw=read --bs=256k --direct=1 --size=1G --numjobs=12 --runtime=30 --group_reporting


# sequential write, 128K block, 16 jobs
fio --name=/dev/sdb --directory=/tmp/fio --ioengine=libaio --iodepth=128 -rw=write --bs=128k --direct=1 --size=1G --numjobs=16 --runtime=30 --group_reporting

# sequential write, 256K block, 16 jobs
fio --name=/dev/sdb --directory=/tmp/fio --ioengine=libaio --iodepth=128 -rw=write --bs=256k --direct=1 --size=1G --numjobs=16 --runtime=30 --group_reporting


# sequential read, 8K block, 16 jobs
fio --name=/dev/sdb --directory=/tmp/fio --ioengine=libaio --iodepth=128 -rw=read --bs=8k --direct=1 --size=1G --numjobs=16 --runtime=30 --group_reporting

# sequential read, 64K block, 16 jobs
fio --name=/dev/sdb --directory=/tmp/fio --ioengine=libaio --iodepth=128 -rw=read --bs=64k --direct=1 --size=1G --numjobs=16 --runtime=30 --group_reporting

# sequential read, 128K block, 16 jobs
fio --name=/dev/sdb --directory=/tmp/fio --ioengine=libaio --iodepth=128 -rw=read --bs=128k --direct=1 --size=1G --numjobs=16 --runtime=30 --group_reporting

# sequential read, 256K block, 16 jobs
fio --name=/dev/sdb --directory=/tmp/fio --ioengine=libaio --iodepth=128 -rw=read --bs=256k --direct=1 --size=1G --numjobs=16 --runtime=30 --group_reporting


```