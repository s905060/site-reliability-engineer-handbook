# RPM

The following rpm command installs Mysql client package.
```
# rpm -ivh  MySQL-client-3.23.57-1.i386.rpm
Preparing...################################### [100%]
   1:MySQL-client############################## [100%]
```

Query all the RPM Packages using rpm -qa
You can use rpm command to query all the packages installed in your system.
```
# rpm -qa
cdrecord-2.01-10.7.el5
bluez-libs-3.7-1.1
setarch-2.0-1.1
.
.
```
* -q query operation
* -a queries all installed packages

To identify whether a particular rpm package is installed on your system, combine rpm and grep command as shown below. Following command checks whether cdrecord package is installed on your system.
```
# rpm -qa | grep 'cdrecord'
```

Query a Particular RPM Package using rpm -q
The above example lists all currently installed package. After installation of a package to check the installation, you can query a particular package and verify as shown below.
```
# rpm -q MySQL-client
MySQL-client-3.23.57-1
# rpm -q MySQL
package MySQL is not installed
```