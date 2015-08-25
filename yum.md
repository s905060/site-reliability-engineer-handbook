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