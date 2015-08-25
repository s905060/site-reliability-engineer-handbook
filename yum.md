# YUM

Install a package using yum install
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

Uninstall a package using yum remove
To remove a package (along with all its dependencies), use ‘yum remove package’ as shown below.
```
# yum remove  postgresql.x86_64
Package postgresql.x86_64 0:9.0.4-5.fc15 will be erased
Is this ok [y/N]: y
Running Transaction
  Erasing    : postgresql-9.0.4-5.fc15.x86_64       1/1
```

Upgrade an existing package using yum update
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

Display additional information about a package using yum info

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
