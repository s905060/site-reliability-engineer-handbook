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

Grant Permissions to MySQL User
The basic syntax for granting permissions is as follows:
```
GRANT permission ON database.table TO 'user'@'localhost';
```
Here is a short list of commonly used permissions :
```
ALL – Allow complete access to a specific database. If a database is not specified, then allow complete access to the entirety of MySQL.
CREATE – Allow a user to create databases and tables.
DELETE – Allow a user to delete rows from a table.
DROP – Allow a user to drop databases and tables.
EXECUTE – Allow a user to execute stored routines.
GRANT OPTION – Allow a user to grant or remove another user’s privileges.
INSERT – Allow a user to insert rows from a table.
SELECT – Allow a user to select data from a database.
SHOW DATABASES- Allow a user to view a list of all databases.
UPDATE – Allow a user to update rows in a table.
```

Example #1: To grant CREATE permissions for all databases * and all tables * to the user we created in the previous tutorial, testuser , use the following command:
```
GRANT CREATE ON *.* TO 'testuser'@'localhost';
```
Using an asterisk (*) in the place of the database or table is a completely valid option, and implies all databases or all tables.

Example #2: To grant testuser the ability to drop tables in the specific database, tutorial_database , use the DROP permission:
```
GRANT DROP ON tutorial_database.* TO 'testuser'@'localhost';
```
When finished making your permission changes, it’s good practice to reload all the privileges with the flush command!
```
FLUSH PRIVILEGES;
```
View Grants for MySQL User
After you’ve granted permissions to a MySQL user you’ll probably want to double check them. Use the following command to check the grants for testuser :
```
SHOW GRANTS FOR 'testuser'@'localhost';
```
