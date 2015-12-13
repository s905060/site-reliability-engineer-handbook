# How can I manually run a nagios check from the command line?

Try this - (plugin full path) - H (servername) -c (checkname)

```
/usr/lib64/nagios/plugins/check_nrpe -H spc7atc01 -c check_cpu
```

output -

```
OK CPU Load ok.|'5'=4;80;90; '10'=3;80;90; '15'=3;80;90;
```