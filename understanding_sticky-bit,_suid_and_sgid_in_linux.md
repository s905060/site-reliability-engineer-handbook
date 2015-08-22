
#Understanding Sticky-Bit, SUID and SGID in Linux

##Sticky Bit :-

The sticky bit is used to indicate special permissions for files and directories. If a directory with sticky bit enabled, will restricts deletion of file inside it. It can be removed by root, owner of file or who have write permission on it. This is usefull for publically accessible directories like /tmp.

Implementation of Sticky bit on file:
###Method 1:

```# chmod +t tecadmin.txt```
```# ls -l tecadmin.txt```
```-rw-r--r-T 1 root root 0 Mar  8 02:06 tecadmin.txt```


### Mothod 2:

```# chmod 1777 tecadmin.txt```

```# ls -l tecadmin.txt```

```-rwxrwxrwt 1 root root 0 Mar  8 02:06 tecadmin.txt```

In above output it showing sticky bit is set with character t or T in permissions filed. Small t represent that execute permission also enable and capital T represent that execute permission are not enabled.

##SUID ( setuid ) :-

If SUID bit is set on a file and a user executed it. The process will have the same rights as the owner of the file being executed.

For example: passwd command have SUID bit enabled. When a normal user change his password this script update few system files like /etc/passwd and /etc/shadow which can’t be update by non root account. So that passwd command process always run with root user rights.

Implementation of SUID on file:
###Mehtod 1:

```# chmod u+s tecadmin.txt```

```# ls -l tecadmin.txt```

```-rwsr-xr-x 1 root root 0 Mar  8 02:06 tecadmin.txt```

###Method 2:

```# chmod 4655 tecadmin.txt```

```# ls -l tecadmin.txt```

```-rwSr-xr-x 1 root root 0 Mar  8 02:06 tecadmin.txt```

##SGID ( setgid) :-

Same as SUID, The process will have the same group rights of the file being executed. If SGID bit is set on any directory, all sub directories and files created inside will get same group ownership as main directory, it doesn’t matter who is creating.

Implementation of SGID on directory:
```# chmod g+s /test/```

```# ls -ld /test```

```drwxrwsrwx 2 root root 4096 Mar  8 03:12 /test```

Now swich to other user and create a file in /test directory.

```# su - tecadmin```

```$ cd /test/```

```$ touch tecadmin.net.txt```

```$ ls -l tecadmin.net.txt```

```-rw-rw-r-- 1 tecadmin root 0 Mar  8 03:13 tecadmin.net.txt```

In above example tecadmin.net.txt is created with root group ownership.

Thanks for reading this article, I hope it will help you to understand sticky bit, suid and sgid in Linux.