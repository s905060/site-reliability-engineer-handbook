# Insertion Sort

The basic operation of insertion sort is to be inserted into a data already sorted data in order to obtain a new number plus one ordered data, the algorithm is suitable for sorting small amount of data, the time complexity is O ( n ^ 2). Stable sorting method. Insertion algorithm to sort the array is divided into two parts: The first part contains all the elements of the array, but the elements except the last one (to make an array of more than a space only insert position), while the second part contains only one element of this (ie, the elements to be inserted). After the first part of the sorting is completed, then the last element has been sorted into the first part.

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