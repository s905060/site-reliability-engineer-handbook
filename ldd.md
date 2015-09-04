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