# Merge Sort

Merge sort is based on the operation of an efficient merge sort algorithm, which is a very typical application of divide and conquer (Divide and Conquer) adoption. The merger has been ordered sequences, fully ordered sequence; which is to make each child an orderly sequence, and then make inter-segment orderly sequence. If two ordered tables are combined into a sorted list, called Road merge.

Merging process: Compare a [i] and a [j] size, if a [i] ≤a [j], then the first element of an ordered list a [i] is copied to r [k] in and so i and k, respectively, plus 1; otherwise, the second ordered list of elements a [j] is copied to r [k] in, and let j and k, respectively, plus 1, so the cycle continues, until one of them ordered to take complete copy a table and then another table ordered the remaining elements to r from subscript subscript t k to the unit. Merge sort we usually use recursive algorithm, the first to be sorted interval [s, t] to the midpoint of two points, then the left subinterval sort, then sort the right side of the sub-interval, the last section of the left and right interval with a merge operation combined into ordered interval [s, t].

```
def merge(left, right):
i, j = 0, 0
result = []
while i &lt; len(left) and j &lt; len(right):
    if left[i] &lt;= right[j]:
        result.append(left[i])
        i += 1
    else:
        result.append(right[j])
        j += 1
result += left[i:]
result += right[j:]
return result
 
def merge_sort(lists):
    # 归并排序
    if len(lists) &lt;= 1:
        return lists
    num = len(lists) / 2
    left = merge_sort(lists[:num])
    right = merge_sort(lists[num:])
    return merge(left, right)
```