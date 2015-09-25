#  Heap Sort

Heap sort (Heapsort) refers to a sorting algorithm uses the accumulation tree (heap). This data structure design, it is a sort of choice. You can use an array of features to quickly locate the specified index element. Heap into large and small root root heap heap is a complete binary tree. Requires large root heap is the value of each node is not greater than the value of the parent node, i.e., A [PARENT [i]]> = A [i]. In an array of non-descending order, we need to use is the large root heap, because according to the requirements of large root heap shows that the maximum value must be in the top of the heap.

```
# 调整堆
def adjust_heap(lists, i, size):
lchild = 2 * i + 1
rchild = 2 * i + 2
max = i
if i &lt; size / 2:
    if lchild &lt; size and lists[lchild] &gt; lists[max]:
        max = lchild
    if rchild &lt; size and lists[rchild] &gt; lists[max]:
        max = rchild
    if max != i:
        lists[max], lists[i] = lists[i], lists[max]
        adjust_heap(lists, max, size)
 
# 创建堆
def build_heap(lists, size):
    for i in range(0, (size/2))[::-1]:
        adjust_heap(lists, i, size)
 
# 堆排序
def heap_sort(lists):
    size = len(lists)
    build_heap(lists, size)
    for i in range(0, size)[::-1]:
        lists[0], lists[i] = lists[i], lists[0]
        adjust_heap(lists, 0, i)
```