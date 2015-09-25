# Insertion Sort

```
def insert_sort(lists):
# 插入排序
count = len(lists)
for i in range(1, count):
    key = lists[i]
    j = i - 1
    while j &gt;= 0:
        if lists[j] &gt; key:
            lists[j + 1] = lists[j]
            lists[j] = key
        j -= 1
return lists
```