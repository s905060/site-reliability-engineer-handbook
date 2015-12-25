# Identify the most CPU-intensive jobs hadooop

```
! # / Bin / bash 
TOPN = 5  # Here I chose the top 5 
for i in  `PS aux | grep -v CPU | Sort 3 -n -k | awk '{Print $ 2}' | tail - $ {}` TOPN ;  
do 
    grep - Apo 'job_ \ d * _ \ d *'  / proc / $ { i } / cmdline   | Sort | uniq ;  
DONE   | uniq - c
```