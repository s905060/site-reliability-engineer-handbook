# TeraSort benchmark suite

The TeraSort benchmark is probably the most well-known Hadoop benchmark. Back in 2008, Yahoo! set a record by sorting 1 TB of data in 209 seconds – on an Hadoop cluster of 910 nodes as Owen O’Malley of the Yahoo! Grid Computing Team reports. One year later in 2009, Yahoo! set another record by sorting a 1 PB (1’000 TB) of data in 16 hours on an even larger Hadoop cluster of 3800 nodes (it took the same cluster only 62 seconds to sort 1 TB of data, easily beating the previous year’s record!).

Basically, the goal of TeraSort is to sort 1TB of data (or any other amount of data you want) as fast as possible. It is a benchmark that combines testing the HDFS and MapReduce layers of an Hadoop cluster. As such it is not surprising that the TeraSort benchmark suite is often used in practice, which has the added benefit that it allows us – among other things – to compare the results of our own cluster with the clusters of other people. You can use the TeraSort benchmark, for instance, to iron out your Hadoop configuration after your cluster passed a convincing TestDFSIO benchmark first. Typical areas where TeraSort is helpful is to determine whether your map and reduce slot assignments are sound (as they depend on the variables such as the number of cores per TaskTracker node and the available RAM), whether other MapReduce-related parameters such as io.sort.mb and mapred.child.java.opts are set to proper values, or whether the FairScheduler configuration you came up with really behaves as expected.

A full TeraSort benchmark run consists of the following three steps:

1. Generating the input data via TeraGen.
2. Running the actual TeraSort on the input data.
3. Validating the sorted output data via TeraValidate.

You do not need to re-generate input data before every TeraSort run (step 2). So you can skip step 1 (TeraGen) for later TeraSort runs if you are satisfied with the generated data.

Figure 1 shows the basic data flow. We use the included HDFS directory names in the later examples.

![](hadoop-benchmarking-terasort-data-flow1-505x600.png)

Figure 1: Hadoop Benchmarking and Stress Testing: The basic data flow of the TeraSort benchmark suite

The next sections describe each of the three steps in greater detail.

###TeraGen: Generate the TeraSort input data (if needed)

TeraGen (source code) generates random data that can be conveniently used as input data for a subsequent TeraSort run.

The syntax for running TeraGen is as follows:

```
$ hadoop jar hadoop-*examples*.jar teragen <number of 100-byte rows> <output dir>
```

Using the HDFS output directory /user/hduser/terasort-input as an example, the command to run TeraGen in order to generate 1TB of input data (i.e. 1,000,000,000,000 bytes) is:

```
$ hadoop jar hadoop-*examples*.jar teragen 10000000000 /user/hduser/terasort-input
```

Please note that the first parameter supplied to TeraGen is 10 billion (10,000,000,000), i.e. not 1 trillion = 1 TB (1,000,000,000,000). The reason is that the first parameter specifies the number of rows of input data to generate, each of which having a size of 100 bytes.

Here is the actual TeraGen data format per row to clear things up:

```
<10 bytes key><10 bytes rowid><78 bytes filler>\r\n
```

where

1. The keys are random characters from the set ‘ ‘ .. ‘~’.
2. The rowid is the right justified row id as a int.
3. The filler consists of 7 runs of 10 characters from ‘A’ to ‘Z’.

If you have a very fast cluster, your map tasks might finish in a few seconds if you use a relatively small default HDFS block size such as 128MB (which is not a bad idea though in general). This means that the time to start/stop map tasks might be larger than the time to perform the actual task. In other words, the overhead for managing the TaskTrackers might exceed the job’s “payload”. An easy way to work around this is to increase the HDFS block size for files written by TeraGen. Keep in mind that the HDFS block size is a per-file setting, and the value specified by the dfs.block.size property in hdfs-default.xml (or conf/hdfs-site.xml if you use a custom configuration file) is just a default value. So if, for example, you want to use an HDFS block size of 512MB (i.e. 536870912 bytes) for the TeraSort benchmark suite, overwrite dfs.block.size when running TeraGen:

```
$ hadoop jar hadoop-*examples*.jar teragen -D dfs.block.size=536870912 ...
```

###TeraSort: Run the actual TeraSort benchmark

TeraSort (source code) is implemented as a MapReduce sort job with a custom partitioner that uses a sorted list of n-1 sampled keys that define the key range for each reduce.

The syntax to run the TeraSort benchmark is as follows:

```
$ hadoop jar hadoop-*examples*.jar terasort <input dir> <output dir>
```

Using the input directory /user/hduser/terasort-input and the output directory /user/hduser/terasort-output as an example, the command to run the TeraSort benchmark is:

```
$ hadoop jar hadoop-*examples*.jar terasort /user/hduser/terasort-input /user/hduser/terasort-output
```

###TeraValidate: Validate the sorted output data of TeraSort

TeraValidate (source code) ensures that the output data of TeraSort is globally sorted.

TeraValidate creates one map task per file in TeraSort’s output directory. A map task ensures that each key is less than or equal to the previous one. The map task also generates records with the first and last keys of the file, and the reduce task ensures that the first key of file i is greater than the last key of file i-1. The reduce tasks do not generate any output if everything is properly sorted. If they do detect any problems, they output the keys that are out of order.

The syntax to run the TeraValidate test is as follows:

```
$ hadoop jar hadoop-*examples*.jar teravalidate <terasort output dir (= input data)> <teravalidate output dir>
```

Using the output directory /user/hduser/terasort-output from the previous sections and the report (output) directory /user/hduser/terasort-validate as an example, the command to run the TeraValidate test is:

```
$ hadoop jar hadoop-*examples*.jar teravalidate /user/hduser/terasort-output /user/hduser/terasort-validate</pre>
```

###Further tips and tricks

Hadoop provides a very convenient way to access statistics about a job from the command line:

```
$ hadoop job -history all <job output directory>
```

This command retrieves a job’s history files (two files that are by default stored in <job output directory>/_logs/history) and computes job statistics from them:

```
$ hadoop fs -ls /user/hduser/terasort-input/_logs/history
Found 2 items
-rw-r--r--   3 hadoop supergroup      17877 2011-02-11 11:58 /user/hduser/terasort-input/_logs/history/master_1297410440884_job_201102110201_0014_conf.xml
-rw-r--r--   3 hadoop supergroup      36758 2011-02-11 11:58 /user/hduser/terasort-input/_logs/history/master_1297410440884_job_201102110201_0014_hadoop_TeraGen
```

>Note: Unfortunately, not all benchmarks/tests shipped with Hadoop write such job history log files. The TestDFSIO benchmark, for instance, does not save job history files.

Here is an exemplary snippet of such job statistics for TeraGen from a small cluster:

```
$ hadoop job -history all /user/hduser/terasort-input

Hadoop job: job_201102110201_0014
=====================================
Job tracker host name: master
job tracker start time: Fri Feb 11 2011
User: hadoop
JobName: TeraGen
JobConf: hdfs://master:54310/app/hadoop/tmp/mapred/system/job_201102110201_0014/job.xml
Submitted At: 11-Feb-2011
Launched At: 11-Feb-2011 13:58:14 (0sec)
Finished At: 11-Feb-2011 15:00:56 (1hrs, 2mins, 41sec)
Status: SUCCESS
=====================================

Task Summary
============================
Kind    Total   Successful      Failed  Killed  StartTime       FinishTime

Setup   1       1               0       0       11-Feb-2011 13:58:15    11-Feb-2011 13:58:16 (1sec)
Map     24      24              0       0       11-Feb-2011 13:58:18    11-Feb-2011 15:00:47 (1hrs, 2mins, 28sec)
Reduce  0       0               0       0
Cleanup 1       1               0       0       11-Feb-2011 15:00:50    11-Feb-2011 15:00:53 (2sec)
============================

Analysis
=========

Time taken by best performing map task task_201102110201_0014_m_000006: 59mins, 5sec
Average time taken by map tasks: 1hrs, 1mins, 24sec
Worse performing map tasks:
TaskId          Timetaken
task_201102110201_0014_m_000004 1hrs, 2mins, 24sec
task_201102110201_0014_m_000020 1hrs, 2mins, 19sec
task_201102110201_0014_m_000013 1hrs, 2mins, 9sec
[...]
```

