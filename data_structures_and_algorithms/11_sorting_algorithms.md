# Sorting Algorithms

## What is Sorting?
Arranging elements in a specific order (ascending/descending). Tradeoffs: time, space, stability, adaptivity, simplicity.

**Properties to Know:**
- **Stable** - Equal elements maintain relative order after sort (important if multi-key)
- **In-place** - Uses O(1) extra memory (excluding recursion stack)
- **Adaptive** - Faster if array already partially sorted
- **Comparison-based** vs **Non-comparison** (counting, radix, bucket)

## Time & Space Complexity Summary

| Algorithm | Best | Average | Worst | Space | Stable | In-Place | Notes |
|---|---|---|---|---|---|---|---|
| **Bubble Sort** | O(n) | O(n²) | O(n²) | O(1) | Yes | Yes | Simple, adaptive (if optimized) |
| **Selection Sort** | O(n²) | O(n²) | O(n²) | O(1) | No | Yes | Min find + swap, few swaps |
| **Insertion Sort** | O(n) | O(n²) | O(n²) | O(1) | Yes | Yes | Great for small/mostly sorted |
| **Merge Sort** | O(n log n) | O(n log n) | O(n log n) | O(n) | Yes | No* | Divide & Conquer, stable, predictable |
| **Quick Sort** | O(n log n) | O(n log n) | O(n²) | O(log n) avg | No | Yes | Pivot choice critical, fast in practice |
| **Heap Sort** | O(n log n) | O(n log n) | O(n log n) | O(1) | No | Yes | Heap-based, no recursion deep issues |
| **Counting Sort** | O(n+k) | O(n+k) | O(n+k) | O(k+n) | Yes | No | Non-comparison, integers 0..k |
| **Radix Sort** | O(d(n+k)) | O(d(n+k)) | O(d(n+k)) | O(n+k) | Yes | No | Sort by digits, d passes |
| **Bucket Sort** | O(n+k) | O(n+k) | O(n²) | O(n+k) | Depends | No | Distribute into buckets |

*k = range of values, d = digits/length*

## 1. Bubble Sort
Repeatedly swap adjacent elements if out of order. Largest bubbles to end.

```python
def bubble_sort(arr):
    n = len(arr)
    for i in range(n):
        swapped = False
        for j in range(n-i-1):
            if arr[j] > arr[j+1]:
                arr[j], arr[j+1] = arr[j+1], arr[j]
                swapped = True
        if not swapped: break  # already sorted (adaptive)
    return arr  # O(n²) worst, O(1), stable
```

## 2. Selection Sort
Find min in unsorted, swap to front. Fewest swaps.

```python
def selection_sort(arr):
    n = len(arr)
    for i in range(n):
        min_idx = i
        for j in range(i+1,n):
            if arr[j] < arr[min_idx]:
                min_idx = j
        if min_idx != i:
            arr[i], arr[min_idx] = arr[min_idx], arr[i]
    return arr  # O(n²), O(1), not stable
```

## 3. Insertion Sort
Build sorted left portion, insert next element in correct place. Shifts elements.

```python
def insertion_sort(arr):
    for i in range(1, len(arr)):
        key = arr[i]
        j = i-1
        while j >= 0 and arr[j] > key:
            arr[j+1] = arr[j]
            j -= 1
        arr[j+1] = key
    return arr  # O(n²), O(1), stable, adaptive
```

## 4. Merge Sort (Divide & Conquer)
Split in half, recursively sort, merge two sorted halves. Predictable O(n log n).

```python
def merge_sort(arr):
    if len(arr) <= 1: return arr
    mid = len(arr)//2
    left = merge_sort(arr[:mid])
    right = merge_sort(arr[mid:])
    return merge(left, right)

def merge(left, right):
    res = []
    i=j=0
    while i<len(left) and j<len(right):
        if left[i] <= right[j]:  # <= keeps stable
            res.append(left[i]); i+=1
        else:
            res.append(right[j]); j+=1
    res.extend(left[i:]); res.extend(right[j:])
    return res  # O(n log n), O(n) space, stable
```

## 5. Quick Sort (Divide & Conquer)
Choose pivot, partition into < pivot, == pivot?, > pivot, recurse. Fast average.

```python
def quick_sort(arr):
    if len(arr) <= 1: return arr
    pivot = arr[len(arr)//2]
    left = [x for x in arr if x < pivot]
    mid  = [x for x in arr if x == pivot]
    right= [x for x in arr if x > pivot]
    return quick_sort(left) + mid + quick_sort(right)  # naive, creates lists
```

**In-place Lomuto-like (concept):**
```python
def quicksort_inplace(arr, left=0, right=None):
    if right is None: right = len(arr)-1
    if left < right:
        p = partition(arr,left,right)
        quicksort_inplace(arr,left,p-1)
        quicksort_inplace(arr,p+1,right)
    return arr

def partition(arr,l,r):
    pivot = arr[r]
    i = l
    for j in range(l,r):
        if arr[j] <= pivot:
            arr[i], arr[j] = arr[j], arr[i]
            i += 1
    arr[i], arr[r] = arr[r], arr[i]
    return i
```

**Pivot choice:** first, last, middle, or random (helps avoid O(n²) worst case - already sorted/reverse).

## 6. Heap Sort
Use max-heap, extract max repeatedly to end.

```python
import heapq

def heap_sort(arr):
    # max-heap via negation
    h = [-x for x in arr]
    heapq.heapify(h)
    return [-heapq.heappop(h) for _ in range(len(h))]  # O(n log n), O(n) for copy
```

Or in-place max-heap logic. Native heapq gives easy form.

## When to Use Which?

| Scenario | Recommended | Why |
|---|---|---|
| Small arrays (n < ~50) | Insertion Sort | Simple, adaptive, low overhead |
| Mostly sorted data | Insertion/Bubble (optimized) | O(n) best case |
| General purpose, average case | Quick Sort | Fast in practice, in-place |
| Need guaranteed O(n log n) + stable | Merge Sort | Predictable, stable (linked lists good) |
| Limited memory, need guaranteed worst-case | Heap Sort | O(1) extra (in-place impl), O(n log n) worst |
| Integers, small range k | Counting Sort | O(n+k) beats comparison |
| Large integers/strings by digits | Radix Sort | O(d(n+k)) |
| Online/streaming small ranges | Counting/Bucket | Efficient if uniform |

## Python Built-in
`sorted(arr)` returns new sorted list (uses Timsort = hybrid Merge+Insertion, stable, O(n log n), adaptive). `arr.sort()` in-place.

```python
nums = [3,1,4,1,5,9]
sorted(nums)  # [1,1,3,4,5,9] - new
nums.sort(reverse=True)
```

## Key Takeaways
- O(n²) easy to code, fine small n; avoid large n
- O(n log n) standard for large data
- Non-comparison sorts faster if constraints fit (range small)
- Stability matters for multi-level sorts (sort by city, then by name)
