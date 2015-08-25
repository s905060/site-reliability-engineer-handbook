# Tar

The following command creates a single archive backup file called my_home_directory.tar under /tmp. This archive will contain all the files and subdirectories under /home/jsmith.
* Option c, stands for create an archive.
* Option v stands for verbose mode, displays additional
information while executing the command.
* Option f indicates the archive file name mentioned in the command.
```
# tar cvf /tmp/my_home_directory.tar /home/jsmith
```

### tar command examples

Create a new tar archive.

```
$ tar cvf archive_name.tar dirname/
```

Extract from an existing tar archive.

```
$ tar xvf archive_name.tar
```

View an existing tar archive.

```
$ tar tvf archive_name.tar
```