# Linux date and Unix timstamp conversion

Current time as a Unix timestamp:
```
date +%s
```

Output is as follows:
```
1361542433
```

Convert a Unix timestamp to a specified date:
```
date -d '2013-2-22 22:14' +%s
```

Output is as follows:
```
1361542440B.
```

Convert Unix timestamp to a date and time
Do not specify a date and time format:
```
date -d @1361542596
```

Output is as follows:
```
Fri Feb 22 22:16:36 CST 2013
```

Specify the conversion date format:
```
date -d @1361542596 +"%Y-%m-%d %H:%M:%S"
```

Output is as follows:
```
2013-02-22 22:16:36
```