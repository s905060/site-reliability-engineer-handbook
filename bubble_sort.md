# Bubble Sort

It repeatedly visited the number of columns to be sorted, a comparison of the two elements, if they put them in the wrong order switching over. Working visit to the number of columns is repeated until there is no longer need to swap, that is to say the number of columns has been sorted completed.

```
def bubble_sort(lists):
# 冒泡排序
count = len(lists)
for i in range(0, count):
    for j in range(i + 1, count):
        if lists[i] &gt; lists[j]:
            temp = lists[j]
            lists[j] = lists[i]
            lists[i] = temp
return lists
```