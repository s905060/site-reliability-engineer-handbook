# YUM

### Install a package using yum install
To install a package, do ‘yum install packagename’. This will also identify the dependencies automatically and install them.
The following example installs postgresql package.
```
# yum install postgresql.x86_64
Resolving Dependencies
Install       2 Package(s)
Is this ok [y/N]: y
Running Transaction
Installing : postgresql-libs-9.0.4-5.fc15.x86_64  1/2
Installing : postgresql-9.0.4-5.fc15.x86_64       2/2
```

### Uninstall a package using yum remove
To remove a package (along with all its dependencies), use ‘yum remove package’ as shown below.
```
# yum remove  postgresql.x86_64
Package postgresql.x86_64 0:9.0.4-5.fc15 will be erased
Is this ok [y/N]: y
Running Transaction
  Erasing    : postgresql-9.0.4-5.fc15.x86_64       1/1
```

### Upgrade an existing package using yum update
If you have a older version of a package, use ‘yum update package’ to upgrade it to the latest current version. This will also identify and install all required dependencies.
```
# yum update postgresql.x86_64
```

Search for a package to be installed using yum search
If you don’t know the exact package name to be installed, use ‘yum search keyword’, which will search all the packages that matches the ‘keyword’ and display it.
The following examples searches the yum repository for all the packages that matches the keyword ‘firefox’ and lists the available packages.
```
# yum search firefox
Loaded plugins: langpacks, presto, refresh-packagekit
============== N/S Matched: firefox ======================
firefox.x86_64 : Mozilla Firefox Web browser
gnome-do-plugins-firefox.x86_64
mozilla-firetray-firefox.x86_64
mozilla-adblockplus.noarch : Mozilla Firefox extension
mozilla-noscript.noarch : Mozilla Firefox extension
```
Name and summary matches only, use "search all" for everything.

### Display additional information about a package using yum info

Once you search for a package using yum search, you can use ‘yum info package’ to view additional information about the package.

The following examples displays additional information about the samba-common package.
```
# yum info samba-common.i686
Loaded plugins: langpacks, presto, refresh-packagekit
Available Packages
Name        : samba-common
Arch        : i686
Epoch       : 1
Version     : 3.5.11
Release     : 71.fc15.1
Size        : 9.9 M
Repo        : updates
Summary     : Files used by both Samba servers and clients
URL         : http://www.samba.org/
License     : GPLv3+ and LGPLv3+
Description : Samba-common provides files necessary for
both the server and client
```

### View all available packages using yum list

The following command will list all the packages available in the yum database.
```
# yum list | less
```
### List only the installed packages using yum list installed

### To view all the packages that are installed on your system, execute the following yum command.
```
# yum list installed | less
```

### Which package does a file belong to? – Use yum provides

Use ‘yum provides’ if you like to know which package a particular file belongs to. For example, if you like to know the name of the package that has the /etc/sysconfig/nfs file, do the following.
```
# yum provides /etc/sysconfig/nfs
Loaded plugins: langpacks, presto, refresh-packagekit
1:nfs-utils-1.2.3-10.fc15.x86_64 : NFS utilities and supporting clients and
                                 : daemons for the kernel NFS server
Repo        : fedora
Matched from:
Filename    : /etc/sysconfig/nfs

1:nfs-utils-1.2.4-1.fc15.x86_64 : NFS utilities and supporting clients and
                                : daemons for the kernel NFS server
Repo        : updates
Matched from:
Filename    : /etc/sysconfig/nfs

1:nfs-utils-1.2.4-1.fc15.x86_64 : NFS utilities and supporting clients and
                                : daemons for the kernel NFS server
Repo        : installed
Matched from:
Other       : Provides-match: /etc/sysconfig/nfs
```
### List available software groups using yum grouplist

In yum, several related packages are grouped together in a specific group. Instead of searching and installing all the individual packages that belongs to a specific function, you can simply install the group, which will install all the packages that belongs to the group.

To view all the available software groups execute ‘yum grouplist’ as shown below. The output is listed in three groups–Installed Groups, Installed Language Groups and Available Groups.
```
# yum grouplist

Installed Groups:
   Administration Tools
   Base
   Design Suite
   ....

Installed Language Groups:
   Arabic Support [ar]
   Armenian Support [hy]
   Bengali Support [bn]
   ....

Available Groups:
   Authoring and Publishing
   Books and Guides
   Clustering
   DNS Name Server
   Development Libraries
   Development Tools
   Directory Server
   Dogtag Certificate System
   ...
```

### Install a specific software group using yum groupinstall

To install specific software group, use groupinstall option as shown below. In the following example, ‘DNS Name Server’ group contains bind and bind-chroot.
```
# yum groupinstall 'DNS Name Server'

Dependencies Resolved
Install       2 Package(s)
Is this ok [y/N]: y

Package(s) data still to download: 3.6 M
(1/2): bind-9.8.0-9.P4.fc15.x86_64.rpm             | 3.6 MB     00:15
(2/2): bind-chroot-9.8.0-9.P4.fc15.x86_64.rpm   |  69 kB     00:00
-----------------------------------------------------------------
Total               235 kB/s | 3.6 MB     00:15

Installed:
  bind-chroot.x86_64 32:9.8.0-9.P4.fc15

Dependency Installed:
  bind.x86_64 32:9.8.0-9.P4.fc15

Complete!
Note: You can also install MySQL database using yum groupinstall as we discussed earlier.

11. Upgrade an existing software group using groupupdate

If you’ve already installed a software group using yum groupinstall, and would like to upgrade it to the latest version, use ‘yum groupupdate’ as shown below.

# yum groupupdate 'Graphical Internet'

Dependencies Resolved
Upgrade       5 Package(s)
Is this ok [y/N]: y   

Running Transaction
  Updating   : evolution-data-server-3.0.2-1.fc15.x86_64     1/10
  Updating   : evolution-3.0.2-3.fc15.x86_64                 2/10
  Updating   : evolution-NetworkManager-3.0.2-3.fc15.x86_64  3/10
  Updating   : evolution-help-3.0.2-3.fc15.noarch            4/10
  Updating   : empathy-3.0.2-3.fc15.x86_64                   5/10
  Cleanup    : evolution-NetworkManager-3.0.1-1.fc15.x86_64  6/10
  Cleanup    : evolution-help-3.0.1-1.fc15.noarch            7/10
  Cleanup    : evolution-3.0.1-1.fc15.x86_64                 8/10
  Cleanup    : empathy-3.0.1-3.fc15.x86_64                   9/10
  Cleanup    : evolution-data-server-3.0.1-1.fc15.x86_64     10/10 

Complete!
12. Uninstall a software group using yum groupremove

To delete an existing software group use ‘yum groupremove’ as shown below.

# yum groupremove 'DNS Name Server'
Dependencies Resolved
Remove        2 Package(s)
Is this ok [y/N]: y

Running Transaction
  Erasing    : 32:bind-chroot-9.8.0-9.P4.fc15.x86_64  1/2
  Erasing    : 32:bind-9.8.0-9.P4.fc15.x86_64            2/2 

Complete!
```
### Display your current yum repositories

All yum commands goes against one or more yum repositories. To view all the yum repositories that are configured in your system, do "yum repolist" as shown below.

The following will display only the enabled repositories.
```
# yum repolist
repo id     repo name                        status
fedora      Fedora 15 - x86_64               24,085
updates     Fedora 15 - x86_64 - Updates     5,612
To display all the repositories (both enabled and disabled), use ‘yum repolist all’.
```
```
# yum repolist all
repo id                   repo name                                status
fedora                    Fedora 15 - x86_64                       enabled: 24,085
fedora-debuginfo          Fedora 15 - x86_64 - Debug               disabled
fedora-source             Fedora 15 - Source                       disabled
rawhide-debuginfo         Fedora - Rawhide - Debug                 disabled
rawhide-source            Fedora - Rawhide - Source                disabled
updates                   Fedora 15 - x86_64 - Updates             enabled:  5,612
updates-debuginfo         Fedora 15 - x86_64 - Updates - Debug     disabled
updates-source            Fedora 15 - Updates Source               disabled
updates-testing           Fedora 15 - x86_64 - Test Updates        disabled
updates-testing-debuginfo Fedora 15 - x86_64 - Test Updates Debug  disabled
updates-testing-source    Fedora 15 - Test Updates Source          disabled
```

### To view only the disabled repositories, use 
```
# yum repositories disabled’.
```

### Install from a disabled repositories using yum –enablerepo

By default yum installs only from the enabled repositories. For some reason if you like to install a package from a disabled repositories, use –enablerepo option in the ‘yum install’ as shown below.
```
# yum --enablerepo=fedora-source install vim-X11.x86_64
Dependencies Resolved
Install       1 Package(s)
Is this ok [y/N]: y

Running Transaction
  Installing : 2:vim-X11-7.3.138-1.fc15.x86_64   1/1 

Complete!
```

### Execute yum commands interactively using Yum Shell

Yum provides the interactive shell to run multiple commands as shown below.
```
# yum shell
Setting up Yum Shell
> info samba.x86_64
Available Packages
Name        : samba
Arch        : x86_64
Epoch       : 1
Version     : 3.5.11
Release     : 71.fc15.1
Size        : 4.6 M
Repo        : updates
Summary     : Server and Client software to interoperate with Windows machines
URL         : http://www.samba.org/
License     : GPLv3+ and LGPLv3+
Description :
            : Samba is the suite of programs by which a lot of PC-related
            : machines share files, printers, and other information (such as
            : lists of available files and printers). The Windows NT, OS/2, and
            : Linux operating systems support this natively, and add-on packages
            : can enable the same thing for DOS, Windows, VMS, UNIX of all
            : kinds, MVS, and more. This package provides an SMB/CIFS server
            : that can be used to provide network services to SMB/CIFS clients.
            : Samba uses NetBIOS over TCP/IP (NetBT) protocols and does NOT
            : need the NetBEUI (Microsoft Raw NetBIOS frame) protocol.

> 
Yum can also read commands from a text file and execute it one by one. This is very helpful when you have multiple systems. Instead of executing the same command on all the systems, create a text file with those commands, and use ‘yum shell’ to execute those commands as shown below.

# cat yum_cmd.txt
repolist
info nfs-utils-lib.x86_64

# yum shell yum_cmd.txt 
repo id     repo name                        status
fedora      Fedora 15 - x86_64               24,085
updates     Fedora 15 - x86_64 - Updates     5,612

Available Packages
Name        : nfs-utils-lib
Arch        : x86_64
Version     : 1.1.5
Release     : 5.fc15
Size        : 61 k
Repo        : fedora
Summary     : Network File System Support Library
URL         : http://www.citi.umich.edu/projects/nfsv4/linux/
License     : BSD
Description : Support libraries that are needed by the commands and
            : daemons the nfs-utils rpm.

Leaving Shell
```