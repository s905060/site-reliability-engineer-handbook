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


## 3. Download Source Tar File



Next, download the source tar file for the package that you want to build and save it under SOURCES directory.

For this example, I’ve used the source code of icecase open source application, which is a server software for streaming multi-media. But, the steps are exactly the same for building RPM for any other application. You just have to download the corresponding source code for the RPM that you are trying to build.
```sh
# cd /root/rpmbuild/SOURCES/

# wget http://downloads.xiph.org/releases/icecast/icecast-2.3.3.tar.gz

# ls -l
-rw-r--r--. 1 root root 1161774 Jun 11  2012 icecast-2.3.3.tar.gz
```


## 4. Create the SPEC File



In this step, we direct RPM in the build process by creating a spec file. The spec file usually consists of the following eight different sections:

1. Preamble – The preamble section contains information about the package being built and define any dependencies to the package. In general, the preamble consists of entries, one per line, that start with a tag followed by a colon, and then some information.
2. %prep – In this section, we prepare the software for building process. Any previous builds are removed during this process and the source file(.tar) file is expanded, etc.
3. One more key thing is to understand there are pre-defined macros available to perform various shortcut options to build rpm. You may be using this macros when you try to build any complex packages. In the below example, I have used a macro called %setup which removes any previous builds, untar the source files and changes the ownership of the files. You can also use sh scripts under %prep section to perform this action but %setup macro simplifies the process by using predefined sh scripts.
4. %description – the description section usually contains description about the package.
5. %build – This is the section that is responsible for performing the build. Usually the %build section is an sh script.
6. %install – the % install section is also executed as sh script just like %prep and %build. This is the step that is used for the installation.
7. %files – This section contains the list of files that are part of the package. If the files are not part of the %files section then it wont be available in the package. Complete paths are required and you can set the attributes and ownership of the files in this section.
8. %clean – This section instructs the RPM to clean up any files that are not part of the application’s normal build area. Lets say for an example, If the application creates a temporary directory structure in /tmp/ as part of its build, it will not be removed. By adding a sh script in %clean section, the directory can be removed after the build process is completed.

Here is the SPEC file that I created for the icecast application to build an RPM file.

```sh
# cat /root/rpmbuild/SPECS/icecast.spec
Name:           icecast
Version:        2.3.3
Release:        0
Summary:        Xiph Streaming media server that supports multiple formats.
Group:          Applications/Multimedia
License:        GPL
URL:            http://www.icecast.org/
Vendor:         Xiph.org Foundation team@icecast.org
Source:         http://downloads.us.xiph.org/releases/icecast/%{name}-%{version}.tar.gz
Prefix:         %{_prefix}
Packager: 	Karthik
BuildRoot:      %{_tmppath}/%{name}-root

%description
Icecast is a streaming media server which currently supports Ogg Vorbis
and MP3 audio streams. It can be used to create an Internet radio
station or a privately running jukebox and many things in between.
It is very versatile in that new formats can be added relatively
easily and supports open standards for commuincation and interaction.

%prep
%setup -q -n %{name}-%{version}

%build
CFLAGS="$RPM_OPT_FLAGS" ./configure --prefix=%{_prefix} --mandir=%{_mandir} --sysconfdir=/etc

make

%install
[ "$RPM_BUILD_ROOT" != "/" ] && rm -rf $RPM_BUILD_ROOT

make DESTDIR=$RPM_BUILD_ROOT install
rm -rf $RPM_BUILD_ROOT%{_datadir}/doc/%{name}

%clean
[ "$RPM_BUILD_ROOT" != "/" ] && rm -rf $RPM_BUILD_ROOT

%files
%defattr(-,root,root)
%doc README AUTHORS COPYING NEWS TODO ChangeLog
%doc doc/*.html
%doc doc/*.jpg
%doc doc/*.css
%config(noreplace) /etc/%{name}.xml
%{_bindir}/icecast
%{_prefix}/share/icecast/*

%changelog

In this file, under % prep section you may noticed the macro “%setup -q -n %{name}-%{version}”. This macro executes the following command in the background.

cd /usr/src/redhat/BUILD
rm -rf icecast
gzip -dc /usr/src/redhat/SOURCES/icecast-2.3.3.tar.gz | tar -xvvf -
if [ $? -ne 0 ]; then
  exit $?
fi
cd icecast
cd /usr/src/redhat/BUILD/icecast
chown -R root.root .
chmod -R a+rX,g-w,o-w .
```

In %build section, you will see the CFLAGS with configure options that defines the options that can be using during RPM installation and the prefix option , mandatory directory to be present for the installation and sysconfig directory under which the system files needs to be copied over.

Below that line, you will see the make utility which determines the list of files needs to be compiled and compiles them appropriately.

In % install section, the line below the %install that says “make install” is used to take the binaries compiled from the previous step and installs or copies them to the appropriate locations so they can be accessed.


## 5. Create the RPM File using rpmbuild



Once the SPEC file is ready, you can start building your rpm with rpm –b command. The –b option is used to perform all the phases of the build process. If you see any errors during this phase, then you need to resolve it before re-attempting again. The errors will be usually of library dependencies and you can download and install it as necessary.

```sh
# cd /root/rpmbuild/SPECS

# rpmbuild -ba icecast.spec
Executing(%prep): /bin/sh -e /var/tmp/rpm-tmp.Kohe4t
+ umask 022
+ cd /root/rpmbuild/BUILD
+ cd /root/rpmbuild/BUILD
+ rm -rf icecast-2.3.3
+ /usr/bin/gzip -dc /root/rpmbuild/SOURCES/icecast-2.3.3.tar.gz
+ /bin/tar -xf -
+ STATUS=0
+ '[' 0 -ne 0 ']'
+ cd icecast-2.3.3
+ /bin/chmod -Rf a+rX,u+w,g-w,o-w .
+ exit 0
Executing(%build): /bin/sh -e /var/tmp/rpm-tmp.ynm7H7
+ umask 022
+ cd /root/rpmbuild/BUILD
+ cd icecast-2.3.3
+ CFLAGS='-O2 -g'
+ ./configure --prefix=/usr --mandir=/usr/share/man --sysconfdir=/etc
checking for a BSD-compatible install... /usr/bin/install -c
checking whether build environment is sane... yes
checking for a thread-safe mkdir -p... /bin/mkdir -p
checking for gawk... gawk
checking whether make sets $(MAKE)... yes
checking whether to enable maintainer-specific portions of Makefiles... no
checking for gcc... gcc
..
..
..
Wrote: /root/rpmbuild/SRPMS/icecast-2.3.3-0.src.rpm
Wrote: /root/rpmbuild/RPMS/x86_64/icecast-2.3.3-0.x86_64.rpm
Executing(%clean): /bin/sh -e /var/tmp/rpm-tmp.dzahrv
+ umask 022
+ cd /root/rpmbuild/BUILD
+ cd icecast-2.3.3
+ '[' /root/rpmbuild/BUILDROOT/icecast-2.3.3-0.x86_64 '!=' / ']'
+ rm -rf /root/rpmbuild/BUILDROOT/icecast-2.3.3-0.x86_64
+ exit 0
```

Note: If you are using SuSE Linux, if rpmbuild is not available, try using “rpm -ba” to build the rpm package.

During the above rpmbuild install, you might notice the following error messages:


**Error 1: XSLT configuration could not be found**


```sh
checking for xslt-config... no
configure: error: XSLT configuration could not be found
error: Bad exit status from /var/tmp/rpm-tmp.8J0ynG (%build)
RPM build errors:
    Bad exit status from /var/tmp/rpm-tmp.8J0ynG (%build)
```

**Solution 1: Install libxstl-devel**

For the xslt-config, you need to install libxstl-devel package as shown below.
```sh
yum install libxstl-devel
```

This will also install the following dependencies:

* libgcrypt
* libgcrypt-devel
* libgpg-error-devel

**Error 2: libvorbis Error**

checking for libvorbis... configure: error: must have Ogg Vorbis v1.0 or above installed
error: Bad exit status from /var/tmp/rpm-tmp.m4Gk3f (%build)

**Solution 2: Install libvorbis-devel**

For the Ogg Vorbis v1.0, install the libvorbis-devel package as shown below:

```sh
yum install libvorbis-devel
```

This will also install the following dependencies:

* libogg
* libogg-devel
* libvorbis


## 6. Verify the Source and Binary RPM Files



Once the rpmbuild is completed, you can verify the source rpm and binary rpm is created in the below directories.
```sh
# ls -l /root/rpmbuild/SRPMS/
-rw-r--r-- 1 root root 1162483 Aug 25 15:46 icecast-2.3.3-0.src.rpm

# ls -l /root/rpmbuild/RPMS/x86_64/
-rw-r--r--. 1 root root 349181 Feb  4 12:54 icecast-2.3.3-0.x86_64.rpm
```