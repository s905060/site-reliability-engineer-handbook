# LDD

What is ldd?

ldd is a Linux utility that is used in case a user wants to know the shared library dependencies of an executable or even that of a shared library.


### 5 ldd Examples

1. Basic example to find the dependency of an executable or shared library.
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