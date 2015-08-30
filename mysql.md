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
```
mysql> CREATE USER 'monty'@'localhost' IDENTIFIED BY 'some_pass';
mysql> GRANT ALL PRIVILEGES ON *.* TO 'monty'@'localhost'
    ->     WITH GRANT OPTION;
mysql> CREATE USER 'monty'@'%' IDENTIFIED BY 'some_pass';
mysql> GRANT ALL PRIVILEGES ON *.* TO 'monty'@'%'
    ->     WITH GRANT OPTION;
mysql> CREATE USER 'admin'@'localhost' IDENTIFIED BY 'admin_pass';
mysql> GRANT RELOAD,PROCESS ON *.* TO 'admin'@'localhost';
mysql> CREATE USER 'dummy'@'localhost';
```