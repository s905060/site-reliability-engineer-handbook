# NameNode benchmark (nnbench)

NNBench (see src/test/org/apache/hadoop/hdfs/NNBench.java) is useful for load testing the NameNode hardware and configuration. It generates a lot of HDFS-related requests with normally very small “payloads” for the sole purpose of putting a high HDFS management stress on the NameNode. The benchmark can simulate requests for creating, reading, renaming and deleting files on HDFS.

I like to run this test simultaneously from several machines – e.g. from a set of DataNode boxes – in order to hit the NameNode from multiple locations at the same time.

The syntax of NNBench is as follows:

```
NameNode Benchmark 0.4
Usage: nnbench <options>
Options:
        -operation <Available operations are create_write open_read rename delete. This option is mandatory>
         * NOTE: The open_read, rename and delete operations assume that the files they operate on, are already available. The create_write operation must be run before running the other operations.
        -maps <number of maps. default is 1. This is not mandatory>
        -reduces <number of reduces. default is 1. This is not mandatory>
        -startTime <time to start, given in seconds from the epoch. Make sure this is far enough into the future, so all maps (operations) will start at the same time>. default is launch time + 2 mins. This is not mandatory
        -blockSize <Block size in bytes. default is 1. This is not mandatory>
        -bytesToWrite <Bytes to write. default is 0. This is not mandatory>
        -bytesPerChecksum <Bytes per checksum for the files. default is 1. This is not mandatory>
        -numberOfFiles <number of files to create. default is 1. This is not mandatory>
        -replicationFactorPerFile <Replication factor for the files. default is 1. This is not mandatory>
        -baseDir <base DFS path. default is /becnhmarks/NNBench. This is not mandatory>
        -readFileAfterOpen <true or false. if true, it reads the file and reports the average time to read. This is valid with the open_read operation. default is false. This is not mandatory>
        -help: Display the help statement
```

The following command will run a NameNode benchmark that creates 1000 files using 12 maps and 6 reducers. It uses a custom output directory based on the machine’s short hostname. This is a simple trick to ensure that one box does not accidentally write into the same output directory of another box running NNBench at the same time.

```
$ hadoop jar hadoop-*test*.jar nnbench -operation create_write \
    -maps 12 -reduces 6 -blockSize 1 -bytesToWrite 0 -numberOfFiles 1000 \
    -replicationFactorPerFile 3 -readFileAfterOpen true \
    -baseDir /benchmarks/NNBench-`hostname -s`
```

Note that by default the benchmark waits 2 minutes before it actually starts!