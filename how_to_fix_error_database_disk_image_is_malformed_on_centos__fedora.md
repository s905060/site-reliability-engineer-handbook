# How To Fix “Error: database disk image is malformed” On CentOS / Fedora

Today I have been trying to update my CentOS 6.5 virtual machine using command “yum update”.

```
# yum update
```
Sample output:

```
Loaded plugins: fastestmirror, security
Loading mirror speeds from cached hostfile
 * base: centos.aol.in
 * epel: epel.mirror.net.in
 * extras: centos.aol.in
 * remi: mirror.smartmedia.net.id
 * remi-php55: mirror.smartmedia.net.id
 * rpmforge: mirrors.ispros.com.bd
 * rpmfusion-free-updates: mirror.smartmedia.net.id
 * rpmfusion-nonfree-updates: mirror.smartmedia.net.id
 * updates: centos.aol.in
Error: database disk image is malformed
```
I got a weird error “Error: database disk image is malformed” and my system didn’t get updated.

How do I solve this error?

It was very simple. After a couple of hours hunting for a solution, I got a working solution from Fedora Forums.

To get rid of this above error, enter the following command From Terminal:
```
# yum clean dbcache
```