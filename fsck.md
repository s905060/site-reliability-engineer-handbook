# FSCK

fsck stands for "file system consistency check". On most systems, fsck is run at boot time if certain conditions are detected. Usually, these conditions are:

* A file system is marked as "dirty" — its written state is inconsistent with data that was scheduled to be written; or,
* A file system has been mounted a set number of times without being checked.