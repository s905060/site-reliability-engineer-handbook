# MapReduce benchmark (mrbench)

MRBench (see src/test/org/apache/hadoop/mapred/MRBench.java) loops a small job a number of times. As such it is a very complimentary benchmark to the “large-scale” TeraSort benchmark suite because MRBench checks whether small job runs are responsive and running efficiently on your cluster. It puts its focus on the MapReduce layer as its impact on the HDFS layer is very limited.

This test should be run from a single box (see caveat below). The command syntax can be displayed via mrbench --help:

```
MRBenchmark.0.0.2
Usage: mrbench [-baseDir <base DFS path for output/input, default is /benchmarks/MRBench>]
          [-jar <local path to job jar file containing Mapper and Reducer implementations, default is current jar file>]
          [-numRuns <number of times to run the job, default is 1>]
          [-maps <number of maps for each run, default is 2>]
          [-reduces <number of reduces for each run, default is 1>]
          [-inputLines <number of input lines to generate, default is 1>]
          [-inputType <type of input to generate, one of ascending (default), descending, random>]
          [-verbose]
```

>Important note: In Hadoop 0.20.2, setting the “-baseDir“ parameter has no effect. This means that multiple parallel “MRBench“ runs (e.g. started from different boxes) might interfere with each other. This is a known bug (MAPREDUCE-2398). I have submitted a patch but it has not been integrated yet.

In Hadoop 0.20.2, the parameters default to:

```
-baseDir: /benchmarks/MRBench  [*** see my note above ***]
-numRuns: 1
-maps: 2
-reduces: 1
-inputLines: 1
-inputType: ascending
```

The command to run a loop of 50 small test jobs is:
```
$ hadoop jar hadoop-*test*.jar mrbench -numRuns 50
```

Exemplary output of the above command:
```
DataLines       Maps    Reduces AvgTime (milliseconds)
1               2       1       31414
```
This means that the average finish time of executed jobs was 31 seconds.

