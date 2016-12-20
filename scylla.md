# ScyllaDB

Scylla is a new approach to NoSQL data store design, optimized for modern hardware. The typical design of NoSQL data stores \(left\) consists of a JVM which runs on top of Linux, utilizes the page cache, and uses complex memory allocation strategies to “trick” the JVM garbage collector to avoid stop-the-world pauses. Such a design suffers from sudden latency hiccups, expensive locking, and low throughput due to low processor utilization.

![](/assets/scylladb_architecture.png)

