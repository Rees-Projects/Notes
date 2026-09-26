# Searching Algorithms

## Linear Search (Sequential Search)
Scan array from left to right until find target.

**Complexity:** O(n) time, O(1) space. Works on **unsorted** and sorted.

```python
def linear_search(arr, target):
    for i, v in enumerate(arr):
        if v == target: return i
    return -1
```

**When to use:** Small lists, unsorted, or data not indexable. Simple, no preconditions.

## Binary Search
Divide and conquer on **sorted** array. Compare mid with target, eliminate half each step.

**Complexity:** O(log n) time, O(1) iterative, O(log n) recursive stack space. Requires **sorted** data (ascending).

### Iterative Binary Search

```python
def binary_search(arr, target):
    left, right = 0, len(arr)-1
    while left <= right:
        mid = (left + right) // 2
        if arr[mid] == target: return mid
        elif arr[mid] < target: left = mid + 1
        else: right = mid - 1
    return -1
```

### Recursive Binary Search

```python
def binary_search_rec(arr, target, left=0, right=None):
    if right is None: right = len(arr)-1
    if left > right: return -1
    mid = (left+right)//2
    if arr[mid] == target: return mid
    if arr[mid] < target:
        return binary_search_rec(arr,target,mid+1,right)
    return binary_search_rec(arr,target,left,mid-1)
```

### Variants - Lower/Upper Bound (Common in Interviews)

**Lower bound:** First index where arr[i] >= target
```python
def lower_bound(arr, target):
    left, right = 0, len(arr)
    while left < right:
        mid = (left+right)//2
        if arr[mid] < target: left = mid + 1
        else: right = mid
    return left  # index, may be len(arr)
```

**Upper bound:** First index where arr[i] > target
```python
def upper_bound(arr, target):
    left, right = 0, len(arr)
    while left < right:
        mid = (left+right)//2
        if arr[mid] <= target: left = mid + 1
        else: right = mid
    return left
```

**Find first occurrence:**
```python
def first_index(arr, target):
    res = -1
    l,r = 0,len(arr)-1
    while l<=r:
        m=(l+r)//2
        if arr[m]==target:
            res=m; r=m-1  # go left
        elif arr[m]<target: l=m+1
        else: r=m-1
    return res
```

**Find last occurrence:**
```python
def last_index(arr, target):
    res=-1;l,r=0,len(arr)-1
    while l<=r:
        m=(l+r)//2
        if arr[m]==target:
            res=m; l=m+1  # go right
        elif arr[m]<target: l=m+1
        else: r=m-1
    return res
```

## Jump Search (Block Search)
Works on sorted array. Jump ahead by fixed step `k = sqrt(n)`, find block containing target, do linear search in block.

**Complexity:** O(√n). Better than linear, worse than binary. Good for large arrays on disk (fewer jumps).

```python
import math
def jump_search(arr, target):
    n=len(arr)
    step=int(math.sqrt(n))
    prev=0
    while arr[min(step,n)-1] < target:
        prev=step; step+=int(math.sqrt(n))
        if prev>=n: return -1
    for i in range(prev, min(step,n)):
        if arr[i]==target: return i
    return -1
```

## Interpolation Search
Improved binary search for **uniformly distributed, sorted** data. Estimates position by value range (linear interpolation).

**Complexity:** O(log log n) average (best), O(n) worst (non-uniform). Better than binary if distribution even.

```python
def interpolation_search(arr, target):
    l,r=0,len(arr)-1
    while l<=r and arr[l]<=target<=arr[r]:
        # avoid division by zero
        if arr[r]==arr[l]: return l if arr[l]==target else -1
        pos = l + ((target-arr[l])*(r-l))//(arr[r]-arr[l])
        if arr[pos]==target: return pos
        if arr[pos]<target: l=pos+1
        else: r=pos-1
    return -1
```

## Exponential Search (Unbounded/Infinite Arrays)
Find range where target lies by doubling index (1,2,4,8...), then binary search in [prev, cur].

**Complexity:** O(log n). Useful when array size unknown (streaming) or for very large arrays.

```python
def exponential_search(arr, target):
    if arr and arr[0]==target: return 0
    i=1
    n=len(arr)
    while i<n and arr[i]<=target:
        i <<= 1  # i *= 2
    # binary search in [i//2, min(i,n-1)]
    l=i>>1; r=min(i,n-1)
    # standard binary
    while l<=r:
        m=(l+r)//2
        if arr[m]==target:return m
        if arr[m]<target:l=m+1
        else:r=m-1
    return -1
```

## Ternary Search (For Max/Min of Unimodal or Finding in Sorted with comparisons)
Splits into 3 parts instead of 2. For **sorted array** can find element (less common than binary). More common for **unimodal functions** (find peak/min/max).

**Complexity:** O(log₃ n) ~ O(log n) (slightly more comparisons than binary in practice).

```python
# Ternary search to find value in sorted array
def ternary_search(arr, target):
    l,r=0,len(arr)-1
    while l<=r:
        m1=l+(r-l)//3
        m2=r-(r-l)//3
        if arr[m1]==target:return m1
        if arr[m2]==target:return m2
        if target<arr[m1]: r=m1-1
        elif target>arr[m2]: l=m2+1
        else: l=m1+1; r=m2-1
    return -1
```

## When to Use

| Search | Precondition | Best For |
|---|---|---|
| Linear | None | Tiny/unsorted, no overhead |
| Binary | Sorted | General O(log n), most common |
| Jump | Sorted | Large arrays, disk I/O |
| Interpolation | Sorted + uniform dist | Predictive by values |
| Exponential | Sorted, unbounded size | Unknown size, infinite lists |
| Ternary | Sorted or unimodal | Find extrema or alternate split |

## Key Takeaways
- Binary search is king for sorted arrays (O(log n))
- Know iterative vs recursive tradeoffs (stack)
- Variants (first/last, lower/upper bound) very common in coding
