# Hill Sort

Hill sorting (Shell Sort) is inserted into a sort. Also known as narrow increment sort, it is inserted directly into a more efficient sorting algorithm improved version. Hill sorting non-stable sorting algorithms. The method because DL. Shell made and named in 1959. Shell sort is the subject of the recording by pressing certain incremental grouping, for each group using the direct insertion sort algorithm sort; with incremental decreases, each containing keywords more and more, when reduced to 1:00 increment, just the whole file is divided into a group, the algorithm terminates.

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