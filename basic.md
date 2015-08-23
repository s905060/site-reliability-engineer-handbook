# Basic

### ps command examples

ps command is used to display information about the processes that are running in the system.

While there are lot of arguments that could be passed to a ps command, following are some of the common ones.

To view current running processes.
```
$ ps -ef | more
```
To view current running processes in a tree structure. H option stands for process hierarchy.
```
$ ps -efH | more
```

### free command examples

This command is used to display the free, used, swap memory available in the system.

Typical free command output. The output is displayed in bytes.

```
$ free
             total       used       free     shared    buffers     cached
Mem:       3566408    1580220    1986188          0     203988     902960
-/+ buffers/cache:     473272    3093136
Swap:      4000176          0    4000176
```

If you want to quickly check how many GB of RAM your system has use the -g option. -b option displays in bytes, -k in kilo bytes, -m in mega bytes.

```
$ free -g
             total       used       free     shared    buffers     cached
Mem:             3          1          1          0          0          0
-/+ buffers/cache:          0          2
Swap:            3          0          3
```

If you want to see a total memory ( including the swap), use the -t switch, which will display a total line as shown below.

```
ramesh@ramesh-laptop:~$ free -t
             total       used       free     shared    buffers     cached
Mem:       3566408    1592148    1974260          0     204260     912556
-/+ buffers/cache:     475332    3091076
Swap:      4000176          0    4000176
Total:     7566584    1592148    5974436
```

### top command examples

top command displays the top processes in the system ( by default sorted by cpu usage ). To sort top output by any column, Press O (upper-case O) , which will display all the possible columns that you can sort by as shown below.

```
Current Sort Field:  P  for window 1:Def
Select sort field via field letter, type any other key to return

  a: PID        = Process Id              v: nDRT       = Dirty Pages count
  d: UID        = User Id                 y: WCHAN      = Sleeping in Function
  e: USER       = User Name               z: Flags      = Task Flags
  ........
```
To displays only the processes that belong to a particular user use -u option. The following will show only the top processes that belongs to oracle user.

```
$ top -u oracle
```

### df command examples

Displays the file system disk space usage. By default df -k displays output in bytes.

```
$ df -k
Filesystem           1K-blocks      Used Available Use% Mounted on
/dev/sda1             29530400   3233104  24797232  12% /
/dev/sda2            120367992  50171596  64082060  44% /home
```

df -h displays output in human readable form. i.e size will be displayed in GB’s.

```
ramesh@ramesh-laptop:~$ df -h
Filesystem            Size  Used Avail Use% Mounted on
/dev/sda1              29G  3.1G   24G  12% /
/dev/sda2             115G   48G   62G  44% /home
Use -T option to display what type of file system.

ramesh@ramesh-laptop:~$ df -T
Filesystem    Type   1K-blocks      Used Available Use% Mounted on
/dev/sda1     ext4    29530400   3233120  24797216  12% /
/dev/sda2     ext4   120367992  50171596  64082060  44% /home
```

### kill command examples

Use kill command to terminate a process. First get the process id using ps -ef command, then use kill -9 to kill the running Linux process as shown below. You can also use killall, pkill, xkill to terminate a unix process.

```
$ ps -ef | grep vim
ramesh    7243  7222  9 22:43 pts/2    00:00:00 vim

$ kill -9 7243
```

More kill examples: 4 Ways to Kill a Process – kill, killall, pkill, xkill

### rm command examples

Get confirmation before removing the file.
```
$ rm -i filename.txt
```
It is very useful while giving shell metacharacters in the file name argument.

Print the filename and get confirmation before removing the file.
```
$ rm -i file*
```
Following example recursively removes all files and directories under the example directory. This also removes the example directory itself.
```
$ rm -r example
```

### cp command examples

Copy file1 to file2 preserving the mode, ownership and timestamp.
```
$ cp -p file1 file2
```
Copy file1 to file2. if file2 exists prompt for confirmation before overwritting it.
```
$ cp -i file1 file2
```

### mv command examples

Rename file1 to file2. if file2 exists prompt for confirmation before overwritting it.

```
$ mv -i file1 file2
```

Note: mv -f is just the opposite, which will overwrite file2 without prompting.

mv -v will print what is happening during file rename, which is useful while specifying shell metacharacters in the file name argument.
```
$ mv -v file1 file2
```

### cat command examples

You can view multiple files at the same time. Following example prints the content of file1 followed by file2 to stdout.

```
$ cat file1 file2
```
While displaying the file, following cat -n command will prepend the line number to each line of the output.

```
$ cat -n /etc/logrotate.conf
    1	/var/log/btmp {
    2	    missingok
    3	    monthly
    4	    create 0660 root utmp
    5	    rotate 1
    6	}
```

### mount command examples

To mount a file system, you should first create a directory and mount it as shown below.
```
# mkdir /u01
```
```
# mount /dev/sdb1 /u01
```
You can also add this to the fstab for automatic mounting. i.e Anytime system is restarted, the filesystem will be mounted.
```
/dev/sdb1 /u01 ext2 defaults 0 2
```

### chmod command examples

chmod command is used to change the permissions for a file or directory.

Give full access to user and group (i.e read, write and execute ) on a specific file.
```
$ chmod ug+rwx file.txt
```
Revoke all access for the group (i.e read, write and execute ) on a specific file.

```
$ chmod g-rwx file.txt
```
Apply the file permissions recursively to all the files in the sub-directories.

```
$ chmod -R ug+rwx file.txt
```
More chmod examples: 7 Chmod Command Examples for Beginners

### chown command examples

chown command is used to change the owner and group of a file. \

To change owner to oracle and group to db on a file. i.e Change both owner and group at the same time.

```
$ chown oracle:dba dbora.sh
```
Use -R to change the ownership recursively.

```
$ chown -R oracle:dba /home/oracle
```

### passwd command examples

Change your password from command line using passwd. This will prompt for the old password followed by the new password.

```
$ passwd
```
Super user can use passwd command to reset others password. This will not prompt for current password of the user.

```
# passwd USERNAME
```

Remove password for a specific user. Root user can disable password for a specific user. Once the password is disabled, the user can login without entering the password.

```
# passwd -d USERNAME
```

### mkdir command examples

Following example creates a directory called temp under your home directory.

```
$ mkdir ~/temp
```

Create nested directories using one mkdir command. If any of these directories exist already, it will not display any error. If any of these directories doesn’t exist, it will create them.
```
$ mkdir -p dir1/dir2/dir3/dir4/
```

### uname command examples

Uname command displays important information about the system such as — Kernel name, Host name, Kernel release number,
Processor type, etc.,

Sample uname output from a Ubuntu laptop is shown below.
```
$ uname -a
Linux john-laptop 2.6.32-24-generic #41-Ubuntu SMP Thu Aug 19 01:12:52 UTC 2010 i686 GNU/Linux
```

### whereis command examples

When you want to find out where a specific Unix command exists (for example, where does ls command exists?), you can execute the following command.

```
$ whereis ls
ls: /bin/ls /usr/share/man/man1/ls.1.gz /usr/share/man/man1p/ls.1p.gz
```

When you want to search an executable from a path other than the whereis default path, you can use -B option and give path as argument to it. This searches for the executable lsmk in the /tmp directory, and displays it, if it is available.

```
$ whereis -u -B /tmp -f lsmk
lsmk: /tmp/lsmk
```

### whatis command examples

Whatis command displays a single line description about a command.
```
$ whatis ls
ls		(1)  - list directory contents
```
```
$ whatis ifconfig
ifconfig (8)         - configure a network interface
```

### locate command examples

Using locate command you can quickly search for the location of a specific file (or group of files). Locate command uses the database created by updatedb.

The example below shows all files in the system that contains the word crontab in it.
```
$ locate crontab
/etc/anacrontab
/etc/crontab
/usr/bin/crontab
/usr/share/doc/cron/examples/crontab2english.pl.gz
/usr/share/man/man1/crontab.1.gz
/usr/share/man/man5/anacrontab.5.gz
/usr/share/man/man5/crontab.5.gz
/usr/share/vim/vim72/syntax/crontab.vim
```

### man command examples

Display the man page of a specific command.
```
$ man crontab
```
When a man page for a command is located under more than one section, you can view the man page for that command from a specific section as shown below.
```
$ man SECTION-NUMBER commandname
```
Following 8 sections are available in the man page.

General commands
System calls
C library functions
Special files (usually devices, those found in /dev) and drivers
File formats and conventions
Games and screensavers
Miscellaneous
System administration commands and daemons
For example, when you do whatis crontab, you’ll notice that crontab has two man pages (section 1 and section 5). To view section 5 of crontab man page, do the following.
```
$ whatis crontab
```
crontab (1)          - maintain crontab files for individual users (V3)

crontab (5)          - tables for driving cron
```
$ man 5 crontab
```

### tail command examples

Print the last 10 lines of a file by default.
```
$ tail filename.txt
```
Print N number of lines from the file named filename.txt
```
$ tail -n N filename.txt
```
View the content of the file in real time using tail -f. This is useful to view the log files, that keeps growing. The command can be terminated using CTRL-C.
```
$ tail -f log-file
```
More tail examples: 3 Methods To View tail -f output of Multiple Log Files in One Terminal

### less command examples

less is very efficient while viewing huge log files, as it doesn’t need to load the full file while opening.
```
$ less huge-log-file.log
```
One you open a file using less command, following two keys are very helpful.
```
CTRL+F – forward one window
CTRL+B – backward one window
```

### su command examples

Switch to a different user account using su command. Super user can switch to any other user without entering their password.
```
$ su - USERNAME
```
Execute a single command from a different account name. In the following example, john can execute the ls command as raj username. Once the command is executed, it will come back to john’s account.
```
[john@dev-server]$ su - raj -c 'ls'

[john@dev-server]$
Login to a specified user account, and execute the specified shell instead of the default shell.

$ su -s 'SHELLNAME' USERNAME
```

### mysql command examples

mysql is probably the most widely used open source database on Linux. Even if you don’t run a mysql database on your server, you might end-up using the mysql command ( client ) to connect to a mysql database running on the remote server.

To connect to a remote mysql database. This will prompt for a password.
```
$ mysql -u root -p -h 192.168.1.2
```
To connect to a local mysql database.
```
$ mysql -u root -p
```
If you want to specify the mysql root password in the command line itself, enter it immediately after -p (without any space).

### yum command examples

To install apache using yum.
```
$ yum install httpd
```
To upgrade apache using yum.
```
$ yum update httpd
```
To uninstall/remove apache using yum.
```
$ yum remove httpd
```

### rpm command examples

To install apache using rpm.
```
# rpm -ivh httpd-2.2.3-22.0.1.el5.i386.rpm
```
To upgrade apache using rpm.
```
# rpm -uvh httpd-2.2.3-22.0.1.el5.i386.rpm
```
To uninstall/remove apache using rpm.
```
# rpm -ev httpd
```
More rpm examples: RPM Command: 15 Examples to Install, Uninstall, Upgrade, Query RPM Packages

### ping command examples

Ping a remote host by sending only 5 packets.
```
$ ping -c 5 gmail.com
```
More ping examples: Ping Tutorial: 15 Effective Ping Command Examples

### date command examples

Set the system date:
```
# date -s "01/31/2010 23:59:53"
```
Once you’ve changed the system date, you should syncronize the hardware clock with the system date as shown below.
```
# hwclock –systohc
```
```
# hwclock --systohc –utc
```
### wget command examples

The quick and effective method to download software, music, video from internet is using wget command.
```
$ wget http://prdownloads.sourceforge.net/sourceforge/nagios/nagios-3.2.1.tar.gz
```
Download and store it with a different name.
```
$ wget -O taglist.zip http://www.vim.org/scripts/download_script.php?src_id=7701
```