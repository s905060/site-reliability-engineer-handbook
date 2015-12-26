# Write-through, write-around, write-back: Cache explained

Cache is vital for application deployment, but which one to choose: Write-through, write-around or write-back cache? We examine the options:

Cache, the technique of storing a copy of data temporarily in rapidly-accessible storage media (also known as memory) local to the CPU and separate from bulk storage, has been around for as long as computing itself.

The existence of cache is based on a mismatch between the performance characteristics of core components of computing architectures, namely that bulk storage cannot keep up with the performance requirements of the CPU and application processing.

Today there is a wide range of caching options available – write-through, write-around and write-back cache, plus a number of products built around these – and the array of options makes it daunting to know which to plump for to achieve the best benefit.

This article will explain caching, its benefits, the variants available, the suppliers that provide them and how to implement them, and pitfalls to look out for in doing so.

###Cache benefits

Gaining better application performance is all about reducing latency in accessing data. Improving the time storage I/O takes to complete usually results in faster application performance, as almost all application workload is dependent on I/O operations.

Of course, nowadays you could deploy flash for all data, with its low latency and high performance. But, this isn’t a practical or cost-effective method as most IT environments only have a portion of data active at any one time, otherwise known as the working set. This is where caching comes in.

Caching provides several benefits:

* Latency is reduced for active data, which results in higher performance levels for the application.
* I/O operations to external storage are reduced as much of the I/O is diverted to cache, resulting in lower levels of SAN traffic and contention for the SAN.
* Data can sit permanently on external storage arrays or traditional storage, which maintains the consistency and integrity of the data using features provided by the array, such as snapshots or replication.
* Flash is targeted at just the part of the workload that benefits from lower latency, resulting in a more cost-effective use of high $/TB storage.