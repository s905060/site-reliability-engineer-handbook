# FSCK explained

FSCK (File System ChecK) is a tool for checking and repairing problems with filesystems on Linux.  Generally it is invoked automatically by the system during bootup in response to a number of matching events. These are generally a number of days since the previous check, a response to a detected problem with the filesystem, or because the system crashed or rebooted unexpectedly.

The reason for a check after a set time duration is to enable the system to check for any potentially unnoticed issues that may cause issues later on. These checks are normal and expected, most systems will set a default check duration of every 30 days, and with a server uptime generally being longer than that it’s a fair expectation to have that a reboot may cause a disk check to take place. Now, the problem with this is that FSCK gives no indication of how long this will take beyond a percentage of completion, which bears no link to the amount of time passing. 50% completed will not equate to half the time of the test passing. The length of time this test takes is dependent on the size of the partitions being tested, the read speed of the disk and the number of files stored on the disk. The more files, or size, or the slower the read speed, the longer the check can take, and this can range from a few minutes to a few hours.

So, if you reboot your server, and it doesn’t appear back online within a few minutes, and the support team responds to your request for assistance with an explanation that the server is performing an FSCK, then this is what they mean. This test is read-only and will not attempt to fix any discovered anomalies.

Sometimes events can happen that cause the system to flag a disk because read-only as a potential problem is detected by the system. After a reboot one of the previously defined checks will be forced and, should everything be fine, the system will then start up as normal. If not, and a problem is actually detected with a disk, or the system previously flagged a problem before crashing/rebooting, then FSCK will start a repair process and halt the system boot process until the root password is entered.  This is required to provide FSCK with the relevant permissions it needs to make changes on the disk to correct the problems.  Also in order to make changes to the disk without creating further corruption the partition being checked needs to be unmounted. This means that the system will be offline for the duration of the repair process.  Once the root password is applied, a command prompt will be given to start FSCK, this can be run interactively where the system will prompt for permission for each repair to be made, or it can be told to just repair everything by the use of a few flags.  Once the command is given, FSCK will begin checking the filesystem and run through 5 phases.

**Phase 1** checks blocks and sizes. This step checks the inodes (part of the filesystem that tells the operating system where files are stored on the disk) for inconsistencies.

**Phase 2** checks path names. This step makes sure that the inodes and directories on the disks match up correctly.

**Phase 3** checks connectivity. This step makes sure that all the directories are connected to the filesystem properly.

**Phase 4** checks reference counts. This step compares information from phases 2 and 3 and then corrects discrepancies between the two.

**Phase 5** checks cylinder groups. This step checks the free blocks on the disk and compares with the inode maps for consistency.

Unfortunately there are no indicators of the time this may take beyond FSCK noting which phase it is working on.  Again, the amount of time this may take is dependent on similar factors to the standard FSCK check. The difference is that with this step involving correcting problems on the disk, the amount of time taken is also dependent on the amount of problems detected.

I’ve personally seen a system take two days to pass through an FSCK repair and come out working on the other side, much to the amazement of everyone watching the errors being repaired scroll up the screen. On the flipside I’ve also seen this process take a few minutes.  Be prepared though, as while FSCK can fix problems with the filesystem so that it doesn’t report errors, this process can sometimes leave you with corrupt files,  through which the system can potentially be left unstable or unbootable. Files that FSCK can not put back where they came from get placed in a directory named “lost+found” with a random file name. So, as usual we strongly recommend keeping sufficient backups and not just relying on tools like FSCK to keep your system online.

Once the FSCK process completes repairing the filesystem, the system will reboot and start up as normal.