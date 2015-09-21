# PMAP

### View the memory map of a process

To view the memory map of a process, specify the PID of it. It displays the process name along with the memory map details as shown below.

```
$ pmap 3244       
3244:   man pmap
00314000    108K r-x--  /lib/ld-2.10.1.so
0032f000      4K r----  /lib/ld-2.10.1.so
00330000      4K rw---  /lib/ld-2.10.1.so
0035a000     20K r-x--  /usr/lib/libgdbm.so.3.0.0
0035f000      4K r----  /usr/lib/libgdbm.so.3.0.0
00360000      4K rw---  /usr/lib/libgdbm.so.3.0.0
00bf5000      4K r-x--    [ anon ]
00c83000   1272K r-x--  /lib/tls/i686/cmov/libc-2.10.1.so
00dc1000      4K -----  /lib/tls/i686/cmov/libc-2.10.1.so
00dc2000      8K r----  /lib/tls/i686/cmov/libc-2.10.1.so
00dc4000      4K rw---  /lib/tls/i686/cmov/libc-2.10.1.so
00dc5000     12K rw---    [ anon ]
00e62000     80K r-x--  /lib/libz.so.1.2.3.3
00e76000      4K r----  /lib/libz.so.1.2.3.3
00e77000      4K rw---  /lib/libz.so.1.2.3.3
08048000    164K r-x--  /usr/bin/man
08071000      4K r----  /usr/bin/man
08072000      4K rw---  /usr/bin/man
08073000      4K rw---    [ anon ]
09b3a000    380K rw---    [ anon ]
b78de000      8K rw---    [ anon ]
b78f3000      8K rw---    [ anon ]
bf8c0000     84K rw---    [ stack ]
 total     2192K
```

### View memory map of multiple processes

You can view multiple process memory map by specifying more than one pid in the command line.

```
$ pmap 3327 3353 3360
```

### View extended memory map about a process

You can view memory map about a process in extended format using -x option as

```
$ pmap -x 3401
3401:   man pmap
Address   Kbytes     RSS    Anon  Locked Mode   Mapping
00110000    1272       -       -       - r-x--  libc-2.10.1.so
0024e000       4       -       -       - -----  libc-2.10.1.so
0024f000       8       -       -       - r----  libc-2.10.1.so
00251000       4       -       -       - rw---  libc-2.10.1.so
00252000      12       -       -       - rw---    [ anon ]
00265000     108       -       -       - r-x--  ld-2.10.1.so
00280000       4       -       -       - r----  ld-2.10.1.so
00281000       4       -       -       - rw---  ld-2.10.1.so
003f5000      80       -       -       - r-x--  libz.so.1.2.3.3
00409000       4       -       -       - r----  libz.so.1.2.3.3
0040a000       4       -       -       - rw---  libz.so.1.2.3.3
00553000      20       -       -       - r-x--  libgdbm.so.3.0.0
00558000       4       -       -       - r----  libgdbm.so.3.0.0
00559000       4       -       -       - rw---  libgdbm.so.3.0.0
0056d000       4       -       -       - r-x--    [ anon ]
08048000     164       -       -       - r-x--  man
08071000       4       -       -       - r----  man
08072000       4       -       -       - rw---  man
08073000       4       -       -       - rw---    [ anon ]
09112000     380       -       -       - rw---    [ anon ]
b7849000       8       -       -       - rw---    [ anon ]
b785e000       8       -       -       - rw---    [ anon ]
bf7eb000      84       -       -       - rw---    [ stack ]
-------- ------- ------- ------- -------
total kB    2192       -       -       -
```

#### Syntax and Options
```
pmap [-x|-d] [-q] pid …
pmap -V
```
#### Short Option	Option Description
```
-x	Show the extended format
-d	Show the device format
-q	Do not display some header/footer lines
-V	Displays version of program
```

