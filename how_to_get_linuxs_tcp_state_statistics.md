# How to get Linux's TCP state statistics

Simple command line can be, for example, we want to link state each TCP port 80:

```
netstat - N | grep `hostname -i` : 80  | awk '/ ^ tcp / {++ S [$ NF]} END {for (Key in S) Print Key, S [Key]}'
```
