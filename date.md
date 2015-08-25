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
* %D date (mm/dd/yy)
* %d day of month (01..31)
* %m month (01..12)
* %y last two digits of year (00..99)
* %a locale’s abbreviated weekday name (Sun..Sat)
* %b locale’s abbreviated month name (Jan..Dec)
* %B locale’s full month name, variable length (January..December)
* %A locale’s full weekday name, variable length (Sunday..Saturday)
* %H hour (00..23)
* %I hour (01..12)
* %Y year (1970...)

The following are various ways to display a past date and time:
```
$ date --date='3 seconds ago'
Thu Jan  1 08:27:00 PST 2009
$ date --date="1 day ago"
Wed Dec 31 08:27:13 PST 2008
$ date --date="1 days ago"
Wed Dec 31 08:27:18 PST 2008
$ date --date="1 month ago"
Mon Dec  1 08:27:23 PST 2008
```
