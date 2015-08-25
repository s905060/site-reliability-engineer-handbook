# MySQL

Connect to the MySQL database using the root user and make sure the connection is successful.

```
[local-host]# mysql -u root -p
Enter password:
mysql>
```
Follows the steps below to stop and start MySQL
```
[local-host]# service mysql status
MySQL running (12588)               [ OK ]
[local-host]# service mysql stop
Shutting down MySQL.                [ OK ]
[local-host]# service mysql start
Starting MySQL.                     [ OK ]
```