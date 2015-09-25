# Hill Sort

```
def shell_sort(lists):
# 希尔排序
count = len(lists)
step = 2
group = count / step
while group &gt; 0:
    for i in range(0, group):
        j = i + group
        while j &lt; count:
            k = j - group
            key = lists[j]
            while k &gt;= 0:
                if lists[k] &gt; key:
                    lists[k + group] = lists[k]
                    lists[k] = key
                k -= group
            j += group
    group /= step
return lists
```