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

