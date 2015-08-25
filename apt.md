# APT

apt-cache search: Search Repository Using Package Name
If you are installing Apache 2, you may guess that the package name is apache2. To verify whether it is a valid package name, you may want to search the repository for that particular package name as shown below.
The following example shows how to search the repository for a specific package name.

```
$ apt-cache search ^apache2$
apache2 - Apache HTTP Server metapackage
```

dpkg -l: Is the Package Already Installed?
Before installing a package, you may want to make sure it is not already installed as shown below using dpkg -l command.
```
$ dpkg -l | grep -i apache
```

apt-get install: Install a Package
Finally, install the package using “apt-get install” as shown below.
```
$ apt-cache search ^apache2$
apache2 - Apache HTTP Server metapackage
$ sudo apt-get install apache2
[sudo] password for ramesh:
The following NEW packages will be installed:
  apache2 apache2-mpm-worker apache2-utils
  apache2.2-common libapr1 libaprutil1 libpq5
0 upgraded, 7 newly installed, 0 to remove and 26 not
upgraded.
```

apt-get remove: Delete a Package
Use “apt-get purge” or “apt-get remove” to delete a package as shown below.
```
$ sudo apt-get purge apache2
(or)
$ sudo apt-get remove apache2
```