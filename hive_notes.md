# Hive notes

hive sample test
```
CREATE TABLE words ( word string);
LOAD DATA LOCAL INPATH \'/usr/share/dict/words\' OVERWRITE INTO TABLE words;
SELECT * FROM words WHERE word LIKE \'%hello%\';
DROP TABLE words ;
```

Rename the table to hive

`ALTER TABLE table_name RENAME TO new_table_name;`

Check the partition table

`SHOW PARTITIONS table_name;`

View Hive built-in functions

`show functions;`

View Hive function of the specific use

`desc function extended function name  ; eg.desc function extended trim;`

Set mapreduce memory size

`set mapred.child.java.opts=-Xmx2048m;`

Set output without compression

`set hive.exec.compress.output=false;`

hive using a derby db as metestore time, specify the database location. Add the following to the hive-site.xml in:
```
<property>
    <name>javax.jdo.option.ConnectionURL</name>
    <value>jdbc:derby:;databaseName=/home/leileisyh/metastore_db;create=true</value>
</property>
```

When this hive for the log This is normal. Try local mode failed. Backup to distributed mode. Please add the following configuration at the hive-site.xml, the mandatory use of distributed mode
```
<property>
    <name>hive.exec.mode.local.auto</name>
    <value>false</value>
    <description> Let hive determine whether to run in local mode automatically </description>
</property>
```

View table structure (which can be found in specific HDFS path hive table)

`desc formatted table name`