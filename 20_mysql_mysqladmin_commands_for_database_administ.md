# 20 MySQL (Mysqladmin) Commands for Database Administration in Linux

mysqladmin is a command-line utility the comes with MySQL server and it is used by Database Administrators to perform some basic MySQL tasks easily such as setting root password, changing root password, monitoring mysql processes, reloading privileges, checking server status etc.

In this article we’ve compiled some very useful ‘mysqladmin‘ commands that are used by system/database administrators in their day-to-day work. You must have MySQL server installed on your system to perform these tasks.

If you don’t have MySQL server installed or you are using older version of MySQL server, then we recommend you all to install or update your version by following our below article.

### 1. How to set MySQL Root password?
If you have fresh installation of MySQL server, then it doesn’t required any password to connect it as root user. To set MySQL password for root user, use the following command.

```bash
# mysqladmin -u root password YOURNEWPASSWORD
```

### 2. How to Change MySQL Root password?
If you would like to change or update MySQL root password, then you need to type the following command. For example, say your old password is 123456 and you want to change it with new password say xyz123.
```bash
mysqladmin -u root -p123456 password 'xyz123'
```

### 3. How to check MySQL Server is running?
To find out whether MySQL server is up and running, use the following command.
```bash
# mysqladmin -u root -p ping

Enter password:
mysqld is alive
```

### 4. How to Check which MySQL version I am running?
The following command shows MySQL version along with the current running status .
```bash
# mysqladmin -u root -p version

Enter password:
mysqladmin  Ver 8.42 Distrib 5.5.28, for Linux on i686
Copyright (c) 2000, 2012, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Server version          5.5.28
Protocol version        10
Connection              Localhost via UNIX socket
UNIX socket             /var/lib/mysql/mysql.sock
Uptime:                 7 days 14 min 45 sec

Threads: 2  Questions: 36002  Slow queries: 0  Opens: 15  Flush tables: 1  Open tables: 8  Queries per second avg: 0.059
```

### 5. How to Find out current Status of MySQL server?
To find out current status of MySQL server, use the following command. The mysqladmin command shows the status of uptime with running threads and queries.
```bash
# mysqladmin -u root -ptmppassword status

Enter password:
Uptime: 606704  Threads: 2  Questions: 36003  Slow queries: 0  Opens: 15  Flush tables: 1  Open tables: 8  Queries per second avg: 0.059
```

### 6. How to check status of all MySQL Server Variable’s and value’s?
To check all the running status of MySQL server variables and values, type the following command. The output would be similar to below.
```bash
# mysqladmin -u root -p extended-status

Enter password:
+------------------------------------------+-------------+
| Variable_name                            | Value       |
+------------------------------------------+-------------+
| Aborted_clients                          | 3           |
| Aborted_connects                         | 3           |
| Binlog_cache_disk_use                    | 0           |
| Binlog_cache_use                         | 0           |
| Binlog_stmt_cache_disk_use               | 0           |
| Binlog_stmt_cache_use                    | 0           |
| Bytes_received                           | 6400357     |
| Bytes_sent                               | 2610105     |
| Com_admin_commands                       | 3           |
| Com_assign_to_keycache                   | 0           |
| Com_alter_db                             | 0           |
| Com_alter_db_upgrade                     | 0           |
| Com_alter_event                          | 0           |
| Com_alter_function                       | 0           |
| Com_alter_procedure                      | 0           |
| Com_alter_server                         | 0           |
| Com_alter_table                          | 0           |
| Com_alter_tablespace                     | 0           |
+------------------------------------------+-------------+
```

### 7. How to see all MySQL server Variables and Values?
To see all the running variables and values of MySQL server, use the command as follows.
```bash
# mysqladmin  -u root -p variables

Enter password:
+---------------------------------------------------+----------------------------------------------+
| Variable_name                                     | Value                                        |
+---------------------------------------------------+----------------------------------------------+
| auto_increment_increment                          | 1                                            |
| auto_increment_offset                             | 1                                            |
| autocommit                                        | ON                                           |
| automatic_sp_privileges                           | ON                                           |
| back_log                                          | 50                                           |
| basedir                                           | /usr                                         |
| big_tables                                        | OFF                                          |
| binlog_cache_size                                 | 32768                                        |
| binlog_direct_non_transactional_updates           | OFF                                          |
| binlog_format                                     | STATEMENT                                    |
| binlog_stmt_cache_size                            | 32768                                        |
| bulk_insert_buffer_size                           | 8388608                                      |
| character_set_client                              | latin1                                       |
| character_set_connection                          | latin1                                       |
| character_set_database                            | latin1                                       |
| character_set_filesystem                          | binary                                       |
| character_set_results                             | latin1                                       |
| character_set_server                              | latin1                                       |
| character_set_system                              | utf8                                         |
| character_sets_dir                                | /usr/share/mysql/charsets/                   |
| collation_connection                              | latin1_swedish_ci                            |
+---------------------------------------------------+----------------------------------------------+
```

### 8. How to check all the running Process of MySQL server?
The following command will display all the running process of MySQL database queries.
```bash
# mysqladmin -u root -p processlist

Enter password:
+-------+---------+-----------------+---------+---------+------+-------+------------------+
| Id    | User    | Host            | db      | Command | Time | State | Info             |
+-------+---------+-----------------+---------+---------+------+-------+------------------+
| 18001 | rsyslog | localhost:38307 | rsyslog | Sleep   | 5590 |       |                  |
| 18020 | root    | localhost       |         | Query   | 0    |       | show processlist |
+-------+---------+-----------------+---------+---------+------+-------+------------------+
```

### 9. How to create a Database in MySQL server?
To create a new database in MySQL server, use the command as shown below.
```bash
# mysqladmin -u root -p create databasename

Enter password:
# mysql -u root -p

Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 18027
Server version: 5.5.28 MySQL Community Server (GPL) by Remi

Copyright (c) 2000, 2012, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| databasename       |
| mysql              |
| test               |
+--------------------+
8 rows in set (0.01 sec)

mysql>
```

### 10. How to drop a Database in MySQL server?
To drop a Database in MySQL server, use the following command. You will be asked to confirm press ‘y‘.
```bash
# mysqladmin -u root -p drop databasename

Enter password:
Dropping the database is potentially a very bad thing to do.
Any data stored in the database will be destroyed.

Do you really want to drop the 'databasename' database [y/N] y
Database "databasename" dropped
11. How to reload/refresh MySQL Privileges?
The reload command tells the server to reload the grant tables. The refresh command flushes all tables and reopens the log files.

# mysqladmin -u root -p reload;
# mysqladmin -u root -p refresh
12. How to shutdown MySQL server Safely?
To shutdown MySQL server safely, type the following command.

mysqladmin -u root -p shutdown

Enter password:
You can also use the following commands to start/stop MySQL server.

# /etc/init.d/mysqld stop
# /etc/init.d/mysqld start
```

### 13. Some useful MySQL Flush commands
Following are some useful flush commands with their description.

flush-hosts: Flush all host information from host cache.
flush-tables: Flush all tables.
flush-threads: Flush all threads cache.
flush-logs: Flush all information logs.
flush-privileges: Reload the grant tables (same as reload).
flush-status: Clear status variables.
```bash
# mysqladmin -u root -p flush-hosts
# mysqladmin -u root -p flush-tables
# mysqladmin -u root -p flush-threads
# mysqladmin -u root -p flush-logs
# mysqladmin -u root -p flush-privileges
# mysqladmin -u root -p flush-status
```

### 14. How to kill Sleeping MySQL Client Process?
Use the following command to identify sleeping MySQL client process.
```bash
# mysqladmin -u root -p processlist

Enter password:
+----+------+-----------+----+---------+------+-------+------------------+
| Id | User | Host      | db | Command | Time | State | Info             |
+----+------+-----------+----+---------+------+-------+------------------+
| 5  | root | localhost |    | Sleep   | 14   |       |					 |
| 8  | root | localhost |    | Query   | 0    |       | show processlist |
+----+------+-----------+----+---------+------+-------+------------------+
Now, run the following command with kill and process ID as shown below.

# mysqladmin -u root -p kill 5

Enter password:
+----+------+-----------+----+---------+------+-------+------------------+
| Id | User | Host      | db | Command | Time | State | Info             |
+----+------+-----------+----+---------+------+-------+------------------+
| 12 | root | localhost |    | Query   | 0    |       | show processlist |
+----+------+-----------+----+---------+------+-------+------------------+
If you like to kill multiple process, then pass the process ID‘s with comma separated as shown below.

# mysqladmin -u root -p kill 5,10
```

### 15. How to run multiple mysqladmin commands together?
If you would like to execute multiple ‘mysqladmin‘ commands together, then the command would be like this.
```bash
# mysqladmin  -u root -p processlist status version

Enter password:
+----+------+-----------+----+---------+------+-------+------------------+
| Id | User | Host      | db | Command | Time | State | Info             |
+----+------+-----------+----+---------+------+-------+------------------+
| 8  | root | localhost |    | Query   | 0    |       | show processlist |
+----+------+-----------+----+---------+------+-------+------------------+
Uptime: 3801  Threads: 1  Questions: 15  Slow queries: 0  Opens: 15  Flush tables: 1  Open tables: 8  Queries per second avg: 0.003
mysqladmin  Ver 8.42 Distrib 5.5.28, for Linux on i686
Copyright (c) 2000, 2012, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Server version          5.5.28
Protocol version        10
Connection              Localhost via UNIX socket
UNIX socket             /var/lib/mysql/mysql.sock
Uptime:                 1 hour 3 min 21 sec

Threads: 1  Questions: 15  Slow queries: 0  Opens: 15  Flush tables: 1  Open tables: 8  Queries per second avg: 0.003
```

### 16. How to Connect remote mysql server
To connect remote MySQL server, use the -h (host)  with IP Address of remote machine.
```bash
# mysqladmin  -h 172.16.25.126 -u root -p
```

### 17. How to execute command on remote MySQL server
Let’s say you would like to see the status of remote MySQL server, then the command would be.
```bash
# mysqladmin  -h 172.16.25.126 -u root -p status
```

### 18. How to start/stop MySQL replication on a slave server?
To start/stop MySQL replication on salve server, use the following commands.
```bash
# mysqladmin  -u root -p start-slave
# mysqladmin  -u root -p stop-slave
```

### 19. How to store MySQL server Debug Information to logs?
It tells the server to write debug information about locks in use, used memory and query usage to the MySQL log file including information about event scheduler.
```bash
# mysqladmin  -u root -p debug

Enter password:
```

### 20. How to view mysqladmin options and usage
To find out more options and usage of myslqadmin command use the help command as shown below. It will display a list of available options.
```bash
# mysqladmin --help
We have tried our best to include almost all of ‘mysqladmin‘ commands with their examples in this article, If still, we’ve missed anything, please do let us know via comments and don’t forget to share with your friends.
```