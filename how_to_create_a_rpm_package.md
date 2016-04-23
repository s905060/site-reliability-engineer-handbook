# How to create a RPM Package

Sometimes you might have access to an open source application source code but might not have the RPM file to install it on your system.

In that situation, you can either compile the source code and install the application from source code, or build a RPM file from source code yourself, and use the RPM file to install the application.

There might also be a situation where you want to build a custom RPM package for the application that you developed.

This tutorial explains how to build a RPM package from the source code.

In order to build RPMs, you will need source code, which usually means a compressed tar file that also includes the SPEC file.

The SPEC file typically contains instructions on how to build RPM, what files are part of package and where it should be installed.

The RPM performs the following tasks during the build process.

1. Executes the commands and macros mentioned in the prep section of the spec file.
2. Checks the content of the file list
3. Executes the commands and macros in the build section of the spec file. Macros from the file list is also executed at this step.
4. Creates the binary package file
5. Creates the source package file

Once the RPM executes the above steps, it creates the binary package file and source package file.

The binary package file consists of all source files along with any additional information to install or uninstall the package.

It is usually enabled with all the options for installing the package that are platform specific. Binary package file contain complete applications or libraries of functions compiled for a particular architecture. The source package usually consists of the original compressed tar file, spec file and the patches which are required to create the binary package file.

Let us see how to create a simple source and BIN RPM packages using a tar file.

If you are new to rpm package, you may first want to understand how to use rpm command to install, upgrade and remove packages on CentOS or RedHat.


## 1. Install rpm-build Package



To build an rpm file based on the spec file that we just created, we need to use rpmbuild command.

rpmbuild command is part of rpm-build package. Install it as shown show below.
```sh
# yum install rpm-build
```

rpm-build is dependent on the following package. If you don’t have these installed already, yum will automatically install these dependencies for you.

```sh
elfutils-libelf
rpm
rpm-libs
rpm-python
```


## 2. RPM Build Directories



rpm-build will automatically create the following directory structures that will be used during the RPM build.
```sh
# ls -lF /root/rpmbuild/
drwxr-xr-x. 2 root root 4096 Feb  4 12:21 BUILD/
drwxr-xr-x. 2 root root 4096 Feb  4 12:21 BUILDROOT/
drwxr-xr-x. 2 root root 4096 Feb  4 12:21 RPMS/
drwxr-xr-x. 2 root root 4096 Feb  4 12:21 SOURCES/
drwxr-xr-x. 2 root root 4096 Feb  4 12:21 SPECS/
drwxr-xr-x. 2 root root 4096 Feb  4 12:21 SRPMS/
```

Note: The above directory structure is for both CentOS and RedHat when using rpmbuild package. You can also use /usr/src/redhat directory, but you need to change the topdir parameter accordingly during the rpm build. If you are doing this on SuSE Enterprise Linux, use /usr/src/packages directory.

If you want to use your own directory structure instead of the /root/rpmbuild, you can use one of the following option:

* Use –buildroot option and specify the custom directory during the rpmbuild
* Specify the topdir parameter in the rpmrc file or rpmmacros file.