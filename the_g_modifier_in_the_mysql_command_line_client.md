# The \G modifier in the MySQL command line client

A little publicized, but exceedingly useful feature of the MySQL command line client is the \G modifier. It formats the query output nicely, so you can read through it easier. To use it, you just replace the semi-colon at the end of the query with ‘\G’.

For example, checking the master status:
```
mysql> SHOW MASTER STATUS;
+------------------+----------+--------------+------------------+
| File             | Position | Binlog_Do_DB | Binlog_Ignore_DB |
+------------------+----------+--------------+------------------+
| mysql-bin.000193 |     7061 |              |                  |
+------------------+----------+--------------+------------------+
1 row in set (0.00 sec)

mysql> SHOW MASTER STATUS\G
*************************** 1. row ***************************
            File: mysql-bin.000193
        Position: 7061
    Binlog_Do_DB:
Binlog_Ignore_DB:
1 row in set (0.00 sec)
```

Now try this for the much larger `SHOW SLAVE STATUS`. Or for the enormous `SHOW ENGINE INNODB STATUS`.

As you can see, this is a handy option to make your console output much easier to read.
