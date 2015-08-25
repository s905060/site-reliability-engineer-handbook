# Du

du command (disk usage) will print the file space usage for a particular directory and its subdirectories.

How much space is taken by my home directory and all its subdirectories?

In the following example, option -s stands for summary only. i.e it displays only the total size of /home/jsmith and not the individual sizes of all the subdirectories inside the /home/jsmith. Option -h displays the information in a human readable format. i.e K for KB, M for MB and G for GB. The ~ indicates the user home directory. This command is same as
```
“du -sh /home/jsmith”
# du -sh ~
320M    /home/jsmith
```

To get the subdirectories under /home/jsmith listed, execute the above
command without the s option.