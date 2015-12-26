# Building RPMs

###Basics of Building RPMs

As a rule of thumb do all of the following as a non-root user!

Create the directory tree like the following:
```
    # Create a working dir
    mkdir rpmbuild
    cd rpmbuild
    # Mandatory directories to build the RPM
    mkdir -p {BUILD,SPECS,SOURCES,RPMS,SRPMS}
```

Specify the build area:
```
# Add the following line to ~/.rpmmacros (create it 
# if it doesn't exist)
%_topdir (echo $HOME)/rpmbuild
```

###Macros

The ~/.rpmmacros file contains global macros defined as %{_macroname}. Notice the global macro birthmark of _ preceding the name. These global variables are accessible to all .spec files.

```
Local macros defined in the .spec file take the form of %{macroname}
```

Usually in the preamble section of a spec file you will see something like this:
```
Name:           test
Version:        1.0
Release:        0
...
```

The above fields can then later be referenced as:
```
# Source0: test-1.0.zip
Source0:        %{name}-%{version}.zip
```

###Examples of built-in macros
```
%_prefix /usr
%_sysconfdir /etc
%_localstatedir /var
%_infodir /usr/share/info
%_mandir /usr/share/man
%_initrddir %{_sysconfdir}/rc.d/init.d
%_defaultdocdir %{_usr}/share/doc
```