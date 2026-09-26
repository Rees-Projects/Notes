# Heaps & Priority Queues

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
| **Delete arbitrary** | O(log n) | Find + replace + bubble |

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
heapq.heappush(pq, (1, "urgent"))      # priority 1 higher? min first
heapq.heappush(pq, (3, "low"))
heapq.heappush(pq, (1, "urgent2"))     # tie-breaker by second
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

## Priority Queue (Abstract)
A data structure that retrieves elements by **priority**, not insertion order. Heaps are the most efficient way (O(log n) insert/extract-min/max vs O(n) unsorted list).

**Behavior:** Highest priority removed first. Min-heap often used (smaller number = higher priority).

## Priority Queue Use Cases
- Dijkstra’s shortest path algorithm
- Prim’s MST
- Huffman coding (min-heap for frequencies)
- A* pathfinding
- Task scheduling (CPU jobs by priority)
- Merge k sorted lists (use min-heap of heads)
- Median maintenance, top-k elements

## Example: Merge K Sorted Lists (Concept)
Push heads of k lists into min-heap (O(k)), extract-min to build result, push next from that list (O(log k) per element) → total O(n log k), better than O(nk).

## Java Comparison
- `PriorityQueue<Integer>` - Min-heap by default (natural order)
- `PriorityQueue<Integer>(Collections.reverseOrder())` - Max-heap
- Custom comparator for objects

## Heapsort
Sorting algorithm using heap: build max-heap O(n), repeatedly extract-max to end O(n log n). In-place, O(1) extra space (excluding recursion), unstable. Not cache-best vs quicksort/mergesort usually.

```python
def heapsort(arr):
    import heapq
    heapq.heapify(arr)  # min-heap O(n)
    return [heapq.heappop(arr) for _ in range(len(arr))]  # O(n log n)
```

## Key Takeaways
- Heap = complete binary tree + heap property, array-backed
- O(1) peek, O(log n) insert/pop
- Min-heap (heapq) vs Max-heap (negate)
- Perfect for priority queues, top-k, graph algorithms (Dijkstra)
