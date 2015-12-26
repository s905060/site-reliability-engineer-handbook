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

###Specify the build area:
```
# Add the following line to ~/.rpmmacros (create it 
# if it doesn't exist)
%_topdir (echo $HOME)/rpmbuild
```