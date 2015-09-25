# Radix Sort

Element distribution radix sort (radix sort) are "assigned ordering" (distribution sort), also known as "bucket method" (bucket sort) or bin sort, as the name suggests, it is part of the information through the key will be sorted to some of the "bucket" in order to achieve the effect of sort, radix sort is part of the stability of the sort, the time complexity is O (nlog (r) m), where r is taken to the base, and m is the heap number, at some point, radix sort efficiency than other sort of stability.

```
import math
def radix_sort(lists, radix=10):
    k = int(math.ceil(math.log(max(lists), radix)))
    bucket = [[] for i in range(radix)]
    for i in range(1, k+1):
        for j in lists:
            bucket[j/(radix**(i-1)) % (radix**i)].append(j)
        del lists[:]
        for z in bucket:
            lists += z
            del z[:]
    return lists
```