# 8 Powerful Awk Built-in Variables – FS, OFS, RS, ORS, NR, NF, FILENAME, FNR

This article is part of the on-going Awk Tutorial Examples series. Awk has several powerful built-in variables. There are two types of built-in variables in Awk.

1. Variable which defines values which can be changed such as field separator and record separator.

2. Variable which can be used for processing and reports such as Number of records, number of fields.


### 1. Awk FS Example: Input field separator variable.

Awk reads and parses each line from input based on whitespace character by default and set the variables $1,$2 and etc. Awk FS variable is used to set the field separator for each record. Awk FS can be set to any single character or regular expression. You can use input field separator using one of the following two options:

* Using -F command line option.
* Awk FS can be set like normal variable.

```
Syntax:

$ awk -F 'FS' 'commands' inputfilename

(or)

$ awk 'BEGIN{FS="FS";}'
```

* Awk FS is any single character or regular expression which you want to use as a input field separator.

* Awk FS can be changed any number of times, it retains its values until it is explicitly changed. If you want to change the field separator, its better to change before you read the line. So that change affects the line what you read.

Here is an awk FS example to read the /etc/passwd file which has “:” as field delimiter.

```
$ cat etc_passwd.awk
BEGIN{
FS=":";
print "Name\tUserID\tGroupID\tHomeDirectory";
}
{
	print $1"\t"$3"\t"$4"\t"$6;
}
END {
	print NR,"Records Processed";
}
```
```
$awk -f etc_passwd.awk /etc/passwd
Name    UserID  GroupID        HomeDirectory
gnats	41	41	/var/lib/gnats
libuuid	100	101	/var/lib/libuuid
syslog	101	102	/home/syslog
hplip	103	7	/var/run/hplip
avahi	105	111	/var/run/avahi-daemon
saned	110	116	/home/saned
pulse	111	117	/var/run/pulse
gdm	112	119	/var/lib/gdm
8 Records Processed
```

### 2. Awk OFS Example: Output Field Separator Variable

Awk OFS is an output equivalent of awk FS variable. By default awk OFS is a single space character. Following is an awk OFS example.
```
$ awk -F':' '{print $3,$4;}' /etc/passwd
41 41
100 101
101 102
103 7
105 111
110 116
111 117
112 119
```