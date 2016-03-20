# Package Manager

This guide may help people switching to a dpkg based distribution like Debian from an RPM based one like RedHat.

A quick comparison of rpm and apt/dpkg command-line arguments

List all installed packages:
```
    rpm -qa
    dpkg --list
```
List information about an installed package:
```
    rpm -qi pkgname
    dpkg --status pkgname     (prints a bunch of extra info too)
```
List files in an installed package
```
    rpm -ql pkgname
    dpkg --listfiles pkgname
```
List information about a package on the local hard drive
```
    rpm -qpi file.rpm
    dpkg --info file.deb
```
List files in a package on the local hard drive
```
    rpm -qpl file.rpm
    dpkg --contents file.deb
```
List files in an uninstalled package (depends on dist)

