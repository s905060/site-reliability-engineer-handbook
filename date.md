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
$ date --date="1 year ago"
Tue Jan  1 08:27:28 PST 2008
$ date --date="yesterday"
Wed Dec 31 08:27:34 PST 2008
$ date --date="10 months 2 day ago"
Thu Feb 28 08:27:41 PST 2008
```

The following examples shows how to display a future date and time.
```
$ date
Thu Jan  1 08:30:07 PST 2009
$ date --date='3 seconds'
Thu Jan  1 08:30:12 PST 2009
$ date --date='4 hours'
Thu Jan  1 12:30:17 PST 2009
$ date --date='tomorrow'
Fri Jan  2 08:30:25 PST 2009
$ date --date="1 day"
Fri Jan  2 08:30:31 PST 2009
$ date --date="1 days"
Fri Jan  2 08:30:38 PST 2009
$ date --date="2 days"
Sat Jan  3 08:30:43 PST 2009
$ date --date='1 month'
Sun Feb  1 08:30:48 PST 2009
$ date --date='1 week'
Thu Jan  8 08:30:53 PST 2009
$ date --date="2 months"
Sun Mar  1 08:30:58 PST 2009
$ date --date="2 years"
Sat Jan  1 08:31:03 PST 2011
$ date --date="next day"
Fri Jan  2 08:31:10 PST 2009
$ date --date="-1 days ago"
Fri Jan  2 08:31:15 PST 2009
$ date --date="this Wednesday"
Wed Jan  7 00:00:00 PST 2009
```