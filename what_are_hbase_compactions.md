# What are HBase Compactions?

### The compactions model is changing drastically with CDH 5/HBase 0.96. Here’s what you need to know.

Apache HBase is a distributed data store based upon a log-structured merge tree, so optimal read performance would come from having only one file per store (Column Family). However, that ideal isn’t possible during periods of heavy incoming writes. Instead, HBase will try to combine HFiles to reduce the maximum number of disk seeks needed for a read. This process is called compaction.

Compactions choose some files from a single store in a region and combine them. This process involves reading KeyValues in the input files and writing out any KeyValues that are not deleted, are inside of the time to live (TTL), and don’t violate the number of versions. The newly created combined file then replaces the input files in the region.

Now, whenever a client asks for data, HBase knows the data from the input files are held in one contiguous file on disk — hence only one seek is needed, whereas previously one for each file could be required. But disk IO isn’t free, and without careful attention, rewriting data over and over can lead to some serious network and disk over-subscription. In other words, compaction is about trading some disk IO now for fewer seeks later.

In this post, you will learn more about the use and implications of compactions in CDH 4, as well as changes to the compaction model in CDH 5 (which will be re-based on HBase 0.96).

### Compaction in CDH 4
The ideal compaction would pick the files that will reduce the most seeks in upcoming reads while also choosing files that will need the least amount of IO. Unfortunately, that problem isn’t solvable without knowledge of the future. As such, it’s just an ideal that HBase should strive for and not something that’s ever really attainable.

Instead of the impossible ideal, HBase uses a heuristic to try and choose which files in a store are likely to be good candidates. The files are chosen on the intuition that like files should be combined with like files – meaning, files that are about the same size should be combined.

The default policy in HBase 0.94 (shipping in CDH 4) looks through the list of HFiles, trying to find the first file that has a size less than the total of all files multiplied by hbase.store.compaction.ratio. Once that file is found, the HFile and all files with smaller sequence ids are chosen to be compacted.