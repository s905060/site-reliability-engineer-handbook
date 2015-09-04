# LDD

What is ldd?

ldd is a Linux utility that is used in case a user wants to know the shared library dependencies of an executable or even that of a shared library.


### 5 ldd Examples

* Basic example to find the dependency of an executable or shared library.
```
$ ldd execv
	linux-vdso.so.1 =>  (0x00007fff3e1f3000)
	libc.so.6 => /lib/libc.so.6 (0x00007f621f162000)
	/lib64/ld-linux-x86-64.so.2 (0x00007f621f504000)
```
```
$ ldd libshared.so
	linux-vdso.so.1 =>  (0x00007fff26ac8000)
	libc.so.6 => /lib/libc.so.6 (0x00007ff1df55a000)
	/lib64/ld-linux-x86-64.so.2 (0x00007ff1dfafe000)
```

In the above examples we tried to run the ldd command on an executable ‘execv’ and a shared library ‘libshared.so’ and as you can see that the ldd command output provided the shared library dependencies.

* Produce more information in output

Use -v option for this.

Lets take an example :
```
$ ldd -v libshared.so
	linux-vdso.so.1 =>  (0x00007fff627a8000)
	libc.so.6 => /lib/libc.so.6 (0x00007f1778f70000)
	/lib64/ld-linux-x86-64.so.2 (0x00007f1779514000)

	Version information:
	./libshared.so:
		libc.so.6 (GLIBC_2.2.5) => /lib/libc.so.6
	/lib/libc.so.6:
		ld-linux-x86-64.so.2 (GLIBC_PRIVATE) => /lib64/ld-linux-x86-64.so.2
		ld-linux-x86-64.so.2 (GLIBC_2.3) => /lib64/ld-linux-x86-64.so.2
```

So we see that along with the dependency information, a whole lot of information related to symbol versions etc is produced in output.

* To display unused direct dependencies

Use -u option for this.

Lets take an example :
```
$ ldd -u func
Unused direct dependencies:

	/usr/lib/libstdc++.so.6
	/lib/libm.so.6
	/lib/libgcc_s.so.1
```
So we see that the above output suggests unused dependencies.

* ldd works only on dynamic executables

Use -r option for this.

Lets take an example:
```
$ ldd -r assert.o
	not a dynamic executable
```
As can be seen in the output, a clear message is displayed stating that the supplied file is not a dynamic executable.

* ldd requires full path to the dynamic executable

Till now I have given examples of my custom executables and shared libraries. Lets try ldd on a standard command line executable like ‘ls':

```
$ ldd ls
ldd: ./ls: No such file or directory
```

So we see that ldd complains that it cannot find ‘ls’.

Now lets try with complete path :
```
$ ldd /bin/ls
	linux-vdso.so.1 =>  (0x00007fff16b71000)
	librt.so.1 => /lib/librt.so.1 (0x00007fa1bd775000)
	libselinux.so.1 => /lib/libselinux.so.1 (0x00007fa1bd557000)
	libacl.so.1 => /lib/libacl.so.1 (0x00007fa1bd34e000)
	libc.so.6 => /lib/libc.so.6 (0x00007fa1bcfcb000)
	libpthread.so.0 => /lib/libpthread.so.0 (0x00007fa1bcdae000)
	/lib64/ld-linux-x86-64.so.2 (0x00007fa1bd99c000)
	libdl.so.2 => /lib/libdl.so.2 (0x00007fa1bcba9000)
	libattr.so.1 => /lib/libattr.so.1 (0x00007fa1bc9a4000)
```