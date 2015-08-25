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