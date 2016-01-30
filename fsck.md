# FSCK

fsck stands for "file system consistency check". On most systems, fsck is run at boot time if certain conditions are detected. Usually, these conditions are:

* A file system is marked as "dirty" — its written state is inconsistent with data that was scheduled to be written; or,
* A file system has been mounted a set number of times without being checked.

The fsck command itself interacts with a matching filesystem-specific fsck command created by the filesystem's authors. Regardless of filesystem type, fsck generally has three modes of operation:
* Check for errors, and prompt the user interactively to decide how to resolve individual problems;
* Check for errors, and attempt to fix any errors automatically; or,
* Check for errors, and make no attempt to repair them, but display the errors on standard output.