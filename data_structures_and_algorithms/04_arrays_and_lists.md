# Arrays & Lists

## Arrays
A fixed-size, contiguous block of memory holding elements of same type. Supports **O(1)** random access by index.

**Characteristics:**
- Static size (fixed length) in some languages (Java arrays)
- Homogeneous elements
- Contiguous memory → cache-friendly
- Index-based 0..n-1

## Dynamic Arrays (Lists)
Grow/shrink automatically (Python list, Java ArrayList). When capacity full, typically resize by doubling (amortized O(1) append).

## Common Operations & Complexity

| Operation | Array (static) | Dynamic Array (List) | Notes |
|---|---|---|---|
| Access by index | O(1) | O(1) | arr[i] |
| Search (unsorted) | O(n) | O(n) | Linear scan |
| Search (sorted) | O(log n) | O(log n) | Binary search |
| Insert at end | O(1)* | O(1) amortized | *if space available |
| Insert at front/middle | O(n) | O(n) | Shift elements |
| Delete at end | O(1) | O(1) | |
| Delete at front/middle | O(n) | O(n) | Shift elements |
| Resize | N/A | O(n) | Occasional when growing |

## Python Examples

**Basic array/list operations:**
```python
# List (dynamic array)
nums = [10, 20, 30, 40]
print(nums[0])      # Access O(1)
nums.append(50)     # End insert O(1) amortized
nums.insert(2, 25)  # Middle insert O(n)
nums.pop()          # End delete O(1)
nums.pop(0)         # Front delete O(n) - shifts
del nums[1]         # Middle delete O(n)
```

**Traversal:**
```python
# O(n)
for n in nums:
    print(n)

# With index
for i, n in enumerate(nums):
    print(i, n)
```

**Linear search (unsorted):**
```python
def linear_search(arr, target):
    for i, val in enumerate(arr):
        if val == target:
            return i
    return -1  # O(n)
```

**Binary search (sorted):**
```python
def binary_search(arr, target):
    left, right = 0, len(arr) - 1
    while left <= right:
        mid = (left + right) // 2
        if arr[mid] == target: return mid
        elif arr[mid] < target: left = mid + 1
        else: right = mid - 1
    return -1  # O(log n)
```

**Two pointers example (reverse):**
```python
def reverse_array(arr):
    left, right = 0, len(arr)-1
    while left < right:
        arr[left], arr[right] = arr[right], arr[left]
        left += 1
        right -= 1
    return arr  # O(n) time, O(1) space
```

## Java Comparison

```java
// Static array
int[] arr = new int[5];
arr[0] = 10;

// Dynamic
ArrayList<Integer> list = new ArrayList<>();
list.add(10);        // end
list.add(1, 20);     // middle O(n)
list.get(0);         // O(1)
list.remove(list.size()-1); // end O(1)
list.remove(0);      // front/middle O(n)
```

## When to Use Arrays/Lists
- Need random access by index (O(1))
- Data size mostly known or append-heavy at end
- Avoid frequent insert/delete in middle/front (use Linked List)
