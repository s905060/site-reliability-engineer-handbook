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
