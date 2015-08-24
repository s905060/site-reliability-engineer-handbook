# Stat

Display statistics of a file or directory.
```
$ stat /etc/my.cnf
 File: `/etc/my.cnf'
  Size: 346 Blocks: 16 IO Block: 4096   regular file
Device: 801h/2049d      Inode: 279856      Links: 1
Access: (0644/-rw-r--r--)  Uid: (0/root)   Gid: (0/root)
Access: 2009-01-01 02:58:30.000000000 -0800
Modify: 2006-06-01 20:42:27.000000000 -0700
Change: 2007-02-02 14:17:27.000000000 -0800
$ stat /home/ramesh
 File: `/home/ramesh'
  Size: 4096            Blocks: 8          IO Block: 4096
directory
Device: 803h/2051d      Inode: 5521409     Links: 7
Access: (0755/drwxr-xr-x)  Uid: (401/ramesh)   Gid:
(401/ramesh)
Access: 2009-01-01 12:17:42.000000000 -0800
Modify: 2009-01-01 12:07:33.000000000 -0800
Change: 2009-01-09 12:07:33.000000000 -0800
```

Display the status of the filesystem using option –f
```
$ stat -f /
 File: "/"
    ID: 0        Namelen: 255     Type: ext2/ext3
Blocks: Total: 2579457    Free: 2008027    Available:
1876998    Size: 4096
Inodes: Total: 1310720    Free: 1215892
```