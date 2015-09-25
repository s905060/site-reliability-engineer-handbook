# Quick Sort

Sorting through a trip that will be sorted data is divided into two separate parts, one part of all of the data than the other portion of all data should be small, and then the two parts of the data in this way were quick sort, the entire sorting process can be recursively, in order to reach the entire data into an ordered sequence.

```
def quick_sort(lists, left, right):
# 快速排序
if left &gt;= right:
    return lists
key = lists[left]
low = left
high = right
while left &lt; right:
    while left &lt; right and lists[right] &gt;= key:
        right -= 1
    lists[left] = lists[right]
    while left &lt; right and lists[left] &lt;= key:
        left += 1
    lists[right] = lists[left]
lists[right] = key
quick_sort(lists, low, left - 1)
quick_sort(lists, left + 1, high)
return lists
 
```