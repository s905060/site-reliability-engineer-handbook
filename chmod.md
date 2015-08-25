# Chmod

![](Screen Shot 2015-08-25 at 7.29.56 PM.png)

|Value	|Meaning
|--     | --
|777|(rwxrwxrwx) No restrictions on permissions. Anybody may do anything. Generally not a desirable setting.
|755|(rwxr-xr-x) The file's owner may read, write, and execute the file. All others may read and execute the file. This setting is common for programs that are used by all users.
|700|(rwx------) The file's owner may read, write, and execute the file. Nobody else has any rights. This setting is useful for programs that only the owner may use and must be kept private from others.
|666|(rw-rw-rw-) All users may read and write the file.
|644|(rw-r--r--) The owner may read and write a file, while all others may only read the file. A common setting for data files that everybody may read, but only the owner may change.
|600|(rw-------) The owner may read and write a file. All others have no rights. A common setting for data files that the owner wants to keep private.


### Directory permissions

The chmod command can also be used to control the access permissions for directories. In most ways, the permissions scheme for directories works the same way as they do with files. However, the execution permission is used in a different way. It provides control for access to file listing and other things. Here are some useful settings for directories:

|Value|	Meaning
|--|--
|777|(rwxrwxrwx) No restrictions on permissions. Anybody may list files, create new files in the directory and delete files in the directory. Generally not a good setting.
|755|(rwxr-xr-x) The directory owner has full access. All others may list the directory, but cannot create files nor delete them. This setting is common for directories that you wish to share with other users.
|700|(rwx------) The directory owner has full access. Nobody else has any rights. This setting is useful for directories that only the owner may use and must be kept private from others.


Following are the symbolic representation of three different roles:
* u is for user,
* g is for group,
* and o is for others.

Following are the symbolic representation of three different permissions:
* r is for read permission,
* w is for write permission,
* x is for execute permission.

Add single permission to a file/directory
Changing permission to a single set. + symbol means adding permission. For example, do the following to give execute permission for the user irrespective of anything else:
```
$ chmod u+x filename
```

Add multiple permission to a file/directory
Use comma to separate the multiple permission sets as shown below.
```
$ chmod u+r,g+x filename
```

Remove permission from a file/directory
The following example removes read and write permission for the user.
```
$ chmod u-rx filename
```

Change permission for all roles on a file/directory
The following example assigns execute privilege to user, group and others (basically anybody can execute this file).
```
$ chmod a+x filename
```

Make permission for a file same as another file (using reference)
If you want to change a file permission same as another file, use the reference option as shown below. In this example, file2′s permission will be set exactly same as file1′s permission.
```
$ chmod --reference=file1 file2
```

Apply the permission to all the files under a directory
recursively
Use option -R to change the permission recursively as shown below.
```
$ chmod -R 755 directory-name/
```

Change execute permission only on the directories (files are not affected)
On a particular directory if you have multiple sub-directories and files, the following command will assign execute permission only to all the sub-directories in the current directory (not the files in the current directory).
```
$ chmod u+X *
```

Note: If the files has execute permission already for either the group or others, the above command will assign the execute permission to the user.