# How to create a Debian package


1.- The package creation process


There are several tools involved in this process, used to build, check and sign the package. Debuild is a wrapper that will call them appropriately, so we don't need to do it manually. Here is a brief description of the tools it invokes:
* dpkg-buildpackage: It creates a temporary directory with the package files, building later the .deb package with its content. To work properly, it heavily relies on the files in the special debian subdirectory: control, rules, changelog, etc.
* lintian: Dissects Debian packages trying to find bugs or policy violations.
* debsign: Signs packages (.dsc and .changes files) using GPG or PGP.

2.- Installing necessary software to build our packages
```sh 
 apt-get install dh-make build-essential
 apt-get install devscripts fakeroot debootstrap pbuilder
```

3.- Setting up environment variables
```sh
 DEBEMAIL="your_email_address@domain.com"  
 DEBFULLNAME="Your Name"  
 export DEBEMAIL DEBFULLNAME  
```

4.- Uncompressing our source code (format of the tar.gz file is software-version.tar.gz)

In my case, for the purpose of this little how-to, I will build the Debian package for a simple "hello world" program written in C.
```sh
 root@debian-package:/opt# tar -xvzf hello-0.1.tar.gz   
 hello-0.1/  
 hello-0.1/Makefile  
 hello-0.1/hello_world.c
```

hello_world.c:
```sh
 #include <stdio.h>  
 main ()  
 {  
      printf("Hello World");  
 } 
```

Makefile:
```sh
 DESTDIR=/  
 INSTALL_LOCATION=$(DESTDIR)/usr/  
 CFLAGS:=$(shell dpkg-buildflags --get CFLAGS)  
 LDFLAGS:=$(shell dpkg-buildflags --get LDFLAGS)  
 all: hello_world  
 hello_world: hello_world.o  
      cc $(CFLAGS) $(LDFLAGS) -o $@ hello_world.o  
 install: hello_world_install  
 hello_world_install:  
      mkdir -p $(INSTALL_LOCATION)/bin  
      cp hello_world $(INSTALL_LOCATION)/bin  
      chmod 755 $(INSTALL_LOCATION)/bin/hello_world  
 clean:  
      rm -f *.o hello_world   
 ```
 
There are a two interesting things that we can see in our Makefile:
* It uses DESTDIR variable to support the DESTDIR convention. 
* Dpkg-buildflags is used to get C compiler options (CFLAGS) as well as linker options (LDFLAGS). This complies with the hardening requirements described in Debian documentation.

On the other hand, if your software has any external dependencies, you would need to install those, so you can compile it successfully. Typically you would be able to install them using apt-get.