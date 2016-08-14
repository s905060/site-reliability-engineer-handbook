# How to fix corrupted files for an HBase table


## Recovery instructions:
```bash
switch to hbase user: su hbase
hbase hbck -details to understand the scope of the problem
hbase hbck -fix to try to recover from region-level inconsistencies
hbase hbck -repair tried to auto-repair, but actually increased number of inconsistencies by 1
hbase hbck -fixMeta -fixAssignments
hbase hbck -repair this time tables got repaired
hbase hbck -details to confirm the fix
```
At this point, HBase was healthy, added additional region, and de-referenced corrupted files. However, HDFS still had corrupted files. Since they were no longer referenced by HBase, we deleted them:
```bash
switch to hdfs user: su hdfs
hdfs fsck / to understand the scope of the problem
hdfs fsck / -delete remove corrupted files only
hdfs fsck / to confirm healthy status
```
NOTE: it is important to fully stop the stack to reset caches
(stop all services thrift, hbase, zoo keeper, hdfs and start them again in a reverse order).

[1] Cloudera page for hbck command:
http://www.cloudera.com/content/cloudera/en/documentation/core/latest/topics/admin_hbck_poller.html

