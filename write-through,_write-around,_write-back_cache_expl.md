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

###Write-through, write-around and write-back cache

There are three main caching techniques that can be deployed, each with their own pros and cons.

**Write-through** cache directs write I/O onto cache and through to underlying permanent storage before confirming I/O completion to the host. This ensures data updates are safely stored on, for example, a shared storage array, but has the disadvantage that I/O still experiences latency based on writing to that storage. Write-through cache is good for applications that write and then re-read data frequently as data is stored in cache and results in low read latency.

**Write-around** cache is a similar technique to write-through cache, but write I/O is written directly to permanent storage, bypassing the cache. This can reduce the cache being flooded with write I/O that will not subsequently be re-read, but has the disadvantage is that a read request for recently written data will create a “cache miss” and have to be read from slower bulk storage and experience higher latency.

**Write-back** cache is where write I/O is directed to cache and completion is immediately confirmed to the host. This results in low latency and high throughput for write-intensive applications, but there is data availability exposure risk because the only copy of the written data is in cache. As we will discuss later, suppliers have added resiliency with products that duplicate writes. Users need to consider whether write-back cache solutions offer enough protection as data is exposed until it is staged to external storage. Write-back cache is the best performing solution for mixed workloads as both read and write I/O have similar response time levels.