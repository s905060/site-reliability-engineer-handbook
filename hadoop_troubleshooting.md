# Hadoop Troubleshooting

### What is structured and unstructured data?

Structured data is the data that is easily identifiable as it is organized in a structure. The most common form of structured data is a database where specific information is stored in tables, that is, rows and columns. Unstructured data refers to any data that cannot be identified easily. It could be in the form of images, videos, documents, email, logs and random text. It is not in the form of rows and columns.

### If we want to copy 10 blocks from one machine to another, but another machine can copy only 8.5 blocks, can the blocks be broken at the time of replication?

In HDFS, blocks cannot be broken down. Before copying the blocks from one machine to another, the Master node will figure out what is the actual amount of space required, how many block are being used, how much space is available, and it will allocate the blocks accordingly.

### How indexing is done in HDFS?

Hadoop has its own way of indexing. Depending upon the block size, once the data is stored, HDFS will keep on storing the last part of the data which will say where the next part of the data will be. In fact, this is the base of HDFS.

### If a data Node is full how it’s identified?

When data is stored in datanode, then the metadata of that data will be stored in the Namenode. So Namenode will identify if the data node is full.

### If datanodes increase, then do we need to upgrade Namenode?

While installing the Hadoop system, Namenode is determined based on the size of the clusters. Most of the time, we do not need to upgrade the Namenode because it does not store the actual data, but just the metadata, so such a requirement rarely arise.

### On what basis data will be stored on a rack?

When the client is ready to load a file into the cluster, the content of the file will be divided into blocks. Now the client consults the Namenode and gets 3 datanodes for every block of the file which indicates where the block should be stored. While placing the datanodes, the key rule followed is “for every block of data, two copies will exist in one rack, third copy in a different rack“. This rule is known as “Replica Placement Policy“.

### How do you debug a performance issue or a long running job?

This is an open ended question and the interviewer is trying to see the level of hands-on experience you have in solving production issues. Use your day to day work experience to answer this question. Here are some of the scenarios and responses to help you construct your answer. On a very high level you will follow the below steps.

 Understand the symptom
 Analyze the situation
 Identify the problem areas
 Propose solution

#### Scenario 1 – Job with 100 mappers and 1 reducer takes a long time for the reducer to start after all the mappers are complete. One of the reasons could be that reduce is spending a lot of time copying the map outputs. So in this case we can try couple of things.

1. If possible add a combiner to reduce the amount of output from the mapper to be sent to the reducer
2. Enable map output compression – this will further reduce the size of the outputs to be transferred to the reducer.

#### Scenario 2 – A particular task is using a lot of memory which is causing the slowness or failure, I will look for ways to reduce the memory usage.

1. Make sure the joins are made in an optimal way with memory usage in mind. For e.g. in Pig joins, the LEFT hand side tables are sent to the reducer first and held in memory and the RIGHT most table in streamed to the reducer. So make sure the RIGHT most table is largest of the datasets in the join.
2. We can also increase the memory requirements needed by the map and reduce tasks by setting – mapreduce.map.memory.mb and mapreduce.reduce.memory.mb

#### Scenario 3 – Understanding the data helps a lot in optimizing the way we use the datasets in PIG and HIVE scripts.

1. If you have smaller tables in join, they can be sent to distributed cache and loaded in memory on the Map side and the entire join can be done on the Map side thereby avoiding the shuffle and reduce phase altogether. This will tremendously improve performance. Look up USING REPLICATED in Pig and MAPJOIN or hive.auto.convert.join in Hive
2. If the data is already sorted you can use USING MERGE which will do a Map Only join
3. If the data is bucketted in hive, you may use hive.optimize.bucketmapjoin or
hive.optimize.bucketmapjoin.sortedmerge depending on the characteristics of the data

#### Scenario 4 – The Shuffle process is the heart of a MapReduce program and it can be tweaked for performance improvement.

1. If you see lots of records are being spilled to the disk (check for Spilled Records in the counters in your MapReduce output) you can increase the memory available for Map to perform the Shuffle by increasing the value in io.sort.mb. This will reduce the amount of Map Outputs written to the disk so the sorting of the keys can be performed in memory.
2. On the reduce side the merge operation (merging the output from several mappers) can be done in disk by setting the mapred.inmem.merge.threshold to 0

### Assume you have Research, Marketing and Finance teams funding 60%, 30% and 10% respectively of your Hadoop Cluster. How will you assign only 60% of cluster resources to Research, 30% to Marketing and 10% to Finance during peak load?

Capacity scheduler in Hadoop is designed to support this use case. Capacity scheduler supports hierarchical queues and capacity can be defined for each queue.

For this use case, you would have to define 3 queues under the root queue and give appropriate capacity in % for each queue.

Illustration

Below properties will be defined in capacity-scheduler.xml
```
<property>
<name>yarn.scheduler.capacity.root.queues</name>
<value>research,marketing,finance</value>
</property>

<property>
<name>yarn.scheduler.capacity.research.capacity</name>
<value>60</value>
</property>

<property>
<name>yarn.scheduler.capacity.research.capacity</name>
<value>30</value>
</property>

<property>
<name>yarn.scheduler.capacity.research.capacity</name>
<value>10</value>
</property>
```

### How do you benchmark your Hadoop cluster with tools that come with Hadoop?

** TestDFSIO**

TestDFSIO gives you an understanding of the I/O performance of your cluster. It is a read and write test for HDFS and helpful in identifying performance bottlenecks in your network, hardware and set up of your NameNode and DataNodes.

** NNBench**

NNBench simulate requests for creating, reading, renaming and deleting files on HDFS and is useful for load testing NameNode hardware configuration

** MRBench**

MRBench is a test for the MapReduce layer. It loops a small MapReduce job for a specific number of times and checks the responsiveness and efficiency of the cluster.

Illustration

TestDFSIO write test with 100 files and file size of 100 MB each.
```
$ hadoop jar /dirlocation/hadoop-test.jar TestDFSIO -write -nrFiles 100 -fileSize 100
```
TestDFSIO read test with 100 files and file size of 100 MB each.
```
$ hadoop jar /dirlocation/hadoop-test.jar TestDFSIO -read -nrFiles 100 -fileSize 100
```
MRBench test to run a lob of 50 small test jobs
```
$ hadoop jar /dirlocation/hadoop-test.jar mrbench -numRuns 50
```
NNBench test that creates 1000 files using 12 maps and 6 reducers.
```
$ hadoop jar /dirlocation/hadoop-test.jar nnbench -operation create_write \
-maps 12 -reduces 6 -blockSize 1 -bytesToWrite 0 -numberOfFiles 1000 \
-replicationFactorPerFile 3
```

### Assume you are doing a join and you notice that all but one reducer is running for a long time how do you address the problem in Pig?

Pig collects all of the records for a given key together on a single reducer. In many data sets, there are a few keys that have three or more orders of magnitude more records than other keys. This results in one or two reducers that will take much longer than the rest. To deal with this, Pig provides skew join.

 In the first MapReduce job pig scans the second input and identifies keys that have so many records.
 In the second MapReduce job, it does the actual join.
 For all except the records with the key(s) identified from the first job, pig would do a standard join.
 For the records with keys identified by the second job, bases on how many records were seen for a given key, those records will be split across appropriate number of reducers.
 The other input to the join that is not split, only the keys in question are then then split and then replicated to each reducer that contains that key

Illustration

jnd = join cinfo by city, users by city using ‘skewed’;

### What is the difference between SORT BY and ORDER BY in Hive?

ORDER BY performs a total ordering of the query result set. This means that all the data is passed through a single reducer, which may take an unacceptably long time to execute for larger data sets.

SORT BY orders the data only within each reducer, thereby performing a local ordering, where each reducer’s output will be sorted. You will not achieve a total ordering on the dataset. Better performance is traded for total ordering.

Assume you have a sales table in a company and it has sales entries from salesman around the globe. How do you rank each salesperson by country based on their sales volume in Hive?

Hive support several analytic functions and one of the functions is RANK() and it is designed to do this operation.

Lookup details on other window and analytic functions – https://cwiki.apache.org/confluence/display/Hive/LanguageManual+WindowingAndAnalytics

Illustration
```
Hive>SELECT
rep_name, rep_country, sales_volume,
rank() over (PARTITION BY rep_country ORDER BY sales_volume DESC) as rank
FROM
salesrep;
```

### What is Speculative execution?

A job running on a Hadoop cluster could be divided in to many tasks. In a big cluster some of these tasks could be running slow for various reasons, hardware degradation or software miconfiguration etc. Hadoop initiates a replica of a task when it sees a tasks which is running for sometime and failed to make any progress, on average, as the other tasks from the job. This replica or duplicate exeuction of task is referred to as Speculative Execution.

When a task completes successfully all the duplicate tasks that are running will be killed. So if the original task completes before the speculative task, then the speculative task is killed; on the other hand, if the speculative task finishes first, then the original is killed.


### What is the benefit of using counters in Hadoop?

Counters are a useful for gathering statistics about the job. Assume you have a 100 node cluster and a job with 100 mappers is running in the cluster on 100 different nodes. Lets say you would like to know each time you see a invalid record in your Map phase. You could add a log message in your Mapper so that each time you see an invalid line you can make an entry in the log. But consolidating all the log messages from 100 different nodes will be time consuming. You can use a counter instead and increment the value of the counter every time you see an invalid record. The nice thing about using counters is that is gives you a consolidate value for the whole job rather than showing 100 separate outputs.

### What is the difference between an InputSplit and a Block?

Block is a physical division of data and does not take in to account the logical boundary of records. Meaning you could have a record that started in one block and ends in another block. Where as InputSplit considers the logical boundaries of records as well.

### Can you change the number of mappers to be created for a job in Hadoop?

No. The number of mappers is determined by the no of input splits.

### How do you do a file system check in HDFS?

FSCK command is used to do a file system check in HDFS. It is a very useful command to check the health of the file, block names and block locations.

Illustration
```
hdfs fsck /dir/hadoop-test -files -blocks -locations
```

### What are the parameters of mappers and reducers function?

Map and Reduce method signature tells you a lot about the type of input and ouput your Job will deal with. Assuming you are using TextInputFormat, Map function’s parameters could look like –

LongWritable (Input Key)
Text (Input Value)
Text (Intermediate Key Output)
IntWritable (Intermediate Output)

The four parameters for reduce function could be –

Text (Intermediate Key Output)
IntWritable (Intermediate Value Output)
Text (Final Key Output)
IntWritable (Final Value Output)

### How do you overwrite replication factor?

There are few ways to do this. Look at the below illustration.

Illustration

hadoop fs -setrep -w 5 -R hadoop-test

hadoop fs -Ddfs.replication=5 -cp hadoop-test/test.csv hadoop-test/test_with_rep5.csv


### What are the functions of InputFormat?

Validate input data is present and check input configuration
Create InputSplits from blocks
Create RecordReader implementation to create key/value pairs from the raw InputSplit. These pairs will be sent one by one to their mapper.

### What is a Record Reader?

A RecordReader uses the data within the boundaries created by the input split to generate key/value pairs. Each of the generated Key/value pair will be sent one by one to their mapper.

### What is a sequence file in Hadoop?

Sequence file is used to store binary key/value pairs. Sequence files support splitting even when the data inside the file is compressed which is not possible with a regular compressed file. You can either choose to perform a record level compression in which the value in the key/value pair will be compressed. Or you can also choose to choose at the block level where multiple records will be compressed together.

### What is the difference between Gen1 and Gen2 Hadoop with regards to the Namenode?

In Gen 1 Hadoop, Namenode is the single point of failure. In Gen 2 Hadoop, we have what is known as Active and Passive Namenodes kind of a structure. If the active Namenode fails, passive Namenode takes over the charge.

### What is MapReduce?

Map Reduce is the ‘heart‘ of Hadoop that consists of two parts – ‘map’ and ‘reduce’. Maps and reduces are programs for processing data. ‘Map’ processes the data first to give some intermediate output which is further processed by ‘Reduce’ to generate the final output. Thus, MapReduce allows for distributed processing of the map and reduction operations.

### Can you explain how do ‘map’ and ‘reduce’ work?

Namenode takes the input and divide it into parts and assign them to data nodes. These datanodes process the tasks assigned to them and make a key-value pair and returns the intermediate output to the Reducer. The reducer collects this key value pairs of all the datanodes and combines them and generates the final output.
