# Scripts

```
for i in $(ls); do echo "$i $(ls $i | wc -l)"; done
```
![](Screen Shot 2015-08-23 at 3.51.01 PM.png)

![](Screen Shot 2015-08-23 at 4.17.57 PM.png)

![](Screen Shot 2015-08-23 at 4.34.40 PM.png)

### List of commands you use most often:
```
history | awk '{a[$2]++}END{for(i in a){print a[i] " " i}}' | sort -rn | head
```