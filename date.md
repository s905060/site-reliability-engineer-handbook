# Date

Display Date and Time in a Specific Format

The following are different ways of displaying the current date and time in various formats:

```
$ date
Thu Jan  1 08:19:23 PST 2009
$ date --date="now"
Thu Jan  1 08:20:05 PST 2009
$ date --date="today"
Thu Jan  1 08:20:12 PST 2009
$ date --date='1970-01-01 00:00:01 UTC +5 hours' +%s 
18001
$ date '+Current Date: %m/%d/%y%nCurrent Time:%H:%M:%S'
Current Date: 01/01/09
Current Time:08:21:41
$ date +"%d-%m-%Y"
01-01-2009
$ date +"%d/%m/%Y"
01/01/2009
$ date +"%A,%B %d %Y"
Thursday,January 01 2009
```
* %D
* %d
* %m
* %y
* %a
* %b locale’s abbreviated month name (Jan..Dec)
* %B locale’s full month name, variable length (January..December)
date (mm/dd/yy)
day of month (01..31)
month (01..12)
last two digits of year (00..99)
locale’s abbreviated weekday name (Sun..Sat) locale’s full weekday name, variable length
* %A (Sunday..Saturday)
* %H
* %I
* %Y
hour (00..23) hour (01..12)
year (1970...)