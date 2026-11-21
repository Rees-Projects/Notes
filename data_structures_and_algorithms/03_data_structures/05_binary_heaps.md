# Binary Heaps

> **Layer:** Data structure | **Built from:** an array (viewed as a complete binary tree) | **Implements ADT:** [Priority Queue](../02_abstract_data_types/06_priority_queue.md) | **Also used by:** [Heap sort](../04_algorithms/01_sorting.md#6-heap-sort)

## What is a Heap?
A **complete binary tree** that satisfies the **heap property**. Usually implemented as an **array** (efficient, no node pointers).

**Heap Property:**
- **Max-Heap**: Parent >= Children (root = maximum)
- **Min-Heap**: Parent <= Children (root = minimum)

**Complete Binary Tree**: All levels full except possibly last, filled left-to-right. This makes array representation perfect (no wasted slots).

## Array Representation of Binary Heap
For node at index `i` (0-based):
- Left child = `2*i + 1`
- Right child = `2*i + 2`
- Parent = `(i-1)//2`

Root at index 0.

## Heap Operations & Complexity

| Operation | Time | Notes |
|---|---|---|
| **Peek (get min/max)** | O(1) | Root element |
| **Insert (push)** | O(log n) | Bubble up (heapify up) |
| **Extract (pop min/max)** | O(log n) | Remove root, replace with last, bubble down |
| **Build heap from array** | O(n) | Efficient (not n log n) |
| **Heapify (sift up/down)** | O(log n) | Per node |
| **Delete arbitrary** | O(n) find + O(log n) fix | O(log n) only if you already know its index |

## Python: heapq (Min-Heap Only)
Python's `heapq` module is a **min-heap**. To simulate max-heap, push **negative** values (integers) or use tuples with negated keys.

```python
import heapq

# Min-heap
heap = []
heapq.heappush(heap, 5)
heapq.heappush(heap, 3)
heapq.heappush(heap, 7)
heapq.heappush(heap, 1)
print(heapq.heappop(heap))  # 1 (min)
print(heap[0])              # peek 3 O(1)

# Build heap from list O(n)
nums = [5,3,7,1,9]
heapq.heapify(nums)  # in-place min-heap
print(nums[0])  # 1

# Max-heap using negatives
maxh = []
heapq.heappush(maxh, -10)
heapq.heappush(maxh, -3)
heapq.heappush(maxh, -5)
print(-heapq.heappop(maxh))  # 10

# Heap of tuples (for priority + value)
pq = []
heapq.heappush(pq, (1, "urgent"))      # smallest number pops first = highest priority
heapq.heappush(pq, (3, "low"))
heapq.heappush(pq, (1, "urgent2"))     # tie on priority -> compares 2nd item ("urgent" < "urgent2")
```

**Tie-breaking gotcha:** on equal priorities Python compares the next tuple item. If that item can't be compared (e.g., dicts or custom objects), you get a `TypeError`. Fix: add an increasing counter as a tie-breaker.

```python
import itertools
counter = itertools.count()
heapq.heappush(pq, (1, next(counter), {"task": "a"}))
heapq.heappush(pq, (1, next(counter), {"task": "b"}))  # counter breaks the tie, FIFO among equals
```

## Custom Min-Heap (for understanding)

```python
class MinHeap:
    def __init__(self):
        self.heap = []
    
    def push(self, val):
        self.heap.append(val)
        self._sift_up(len(self.heap)-1)
    
    def pop(self):
        if not self.heap: return None
        if len(self.heap) == 1: return self.heap.pop()
        root = self.heap[0]
        self.heap[0] = self.heap.pop()
        self._sift_down(0)
        return root
    
    def peek(self):
        return self.heap[0] if self.heap else None
    
    def _sift_up(self, i):
        parent = (i-1)//2
        while i > 0 and self.heap[i] < self.heap[parent]:
            self.heap[i], self.heap[parent] = self.heap[parent], self.heap[i]
            i = parent
            parent = (i-1)//2
    
    def _sift_down(self, i):
        n = len(self.heap)
        smallest = i
        left = 2*i+1; right = 2*i+2
        if left < n and self.heap[left] < self.heap[smallest]:
            smallest = left
        if right < n and self.heap[right] < self.heap[smallest]:
            smallest = right
        if smallest != i:
            self.heap[i], self.heap[smallest] = self.heap[smallest], self.heap[i]
            self._sift_down(smallest)
    
    def size(self): return len(self.heap)
```

## Priority Queue Connection
A binary heap is the **data structure**. The Priority Queue is the **ADT** it implements. For the use cases, and for how a heap compares to array or BST implementations, see [Priority Queue](../02_abstract_data_types/06_priority_queue.md).

## Java Comparison
- `PriorityQueue<Integer>` - Min-heap by default (natural order)
- `PriorityQueue<Integer>(Collections.reverseOrder())` - Max-heap
- Custom comparator for objects

## Heapsort
Sorting algorithm using heap: build max-heap O(n), repeatedly swap the max to the end and sift down, O(n log n). The **classic version** is in-place with O(1) extra space, and unstable. Usually slower in practice than quicksort (poor cache locality).

The simple `heapq` version below is easy to write but **not** in-place: it builds a new list, so it uses **O(n) extra space**. Copying first (`h = list(arr)`) avoids destroying the caller's list.

```python
import heapq

def heapsort_simple(arr):
    h = list(arr)          # copy - O(n) space, leaves arr untouched
    heapq.heapify(h)       # min-heap O(n)
    return [heapq.heappop(h) for _ in range(len(h))]  # O(n log n)
```

Full in-place version: see [Sorting - Heap Sort](../04_algorithms/01_sorting.md#6-heap-sort).

## Key Takeaways
- Heap = complete binary tree + heap property, array-backed
- O(1) peek, O(log n) insert/pop
- Min-heap (heapq) vs Max-heap (negate)
- The standard implementation of the Priority Queue ADT (top-k, Dijkstra, scheduling)
