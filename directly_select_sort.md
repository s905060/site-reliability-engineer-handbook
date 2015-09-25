# Directly Select Sort

The basic idea: the first trip, to be sorted in record r 1 ~ r [N] selected smallest recorded it with r 1 exchange; a second trip, to be sorted in record r 2 ~ r [N] selected the smallest recorded it with r 2 switching; and so on, the first record to be sorted at times i r [i] ~ r [n] selected smallest record, it would be exchanged with r [i], so ordered growing sequence until all sorted.

```
def select_sort(lists):
# 选择排序
count = len(lists)
for i in range(0, count):
    min = i
    for j in range(i + 1, count):
        if lists[min] &gt; lists[j]:
            min = j
    temp = lists[min]
    lists[min] = lists[i]
    lists[i] = temp
return lists
```