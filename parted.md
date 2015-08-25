# Parted

Select the hard disk to be parted
When you execute parted command without any argument, by default it selects the first hard disk drive that is available on your system.

In the following example, it picked /dev/sda automatically as it is the first hard drive in this system.
```
# parted
GNU Parted 2.3
Using /dev/sda
Welcome to GNU Parted! Type 'help' to view a list of
commands.
(parted)
To choose a different hard disk, use the select command as
shown below.
(parted) select /dev/sdb
```

It will throw the following error message when it doesn’t find the given hard disk name.
```
Error: Error opening /dev/sdb: No medium found
Retry/Cancel? y
```