# Priority Queue (ADT)

> **Layer:** ADT | **Rule:** highest priority comes out first | **Implemented by:** [Binary heap](../03_data_structures/05_binary_heaps.md) (usually), or a sorted/unsorted array or a BST

## Behavior
Items come out by **priority**, not by the order they arrived. In a min-priority queue the smallest key is served first, and in a max-priority queue the largest. (A normal Queue is FIFO; a priority queue ignores arrival order.)

**Core operations:** `insert(x, priority)`, `extract_min()` / `extract_max()`, `peek()`, `size()`, `is_empty()`

**Common mistake:** "Priority queue" and "heap" are **not** the same thing. The priority queue is the **ADT**. The heap is the **data structure** most often used to implement it.

## Implementations Compared

| Implementation | insert | extract-min | peek | Notes |
|---|---|---|---|---|
| Unsorted array | **O(1)** append | O(n) scan | O(n) | Good if you insert a lot and extract rarely |
| Sorted array | O(n) shift into place | **O(1)** pop end | O(1) | Good if you extract a lot and insert rarely |
| Balanced BST | O(log n) | O(log n) | O(log n) | Also supports deleting any item and in-order iteration |
| **Binary heap** | **O(log n)** | **O(log n)** | **O(1)** | ✅ Best balance, array-backed, simple |

**Why the heap wins:** Both operations are O(log n), so neither one is ever O(n). For n items inserted and then extracted, a heap does O(n log n) total work, while either kind of array does O(n²).

## Python / Java
- Python: `heapq` (a min-heap on a plain `list`). For a max-heap, push `-priority`.
- Java: `PriorityQueue<E>` (min-heap by default; use `Collections.reverseOrder()` for max).

```python
import heapq, itertools
pq, counter = [], itertools.count()
heapq.heappush(pq, (2, next(counter), "write report"))
heapq.heappush(pq, (1, next(counter), "fix outage"))
priority, _, task = heapq.heappop(pq)   # -> (1, ..., "fix outage")
```
(The counter breaks ties so equal priorities come out in FIFO order, and it stops Python from comparing the task objects. More on this in [Binary Heaps](../03_data_structures/05_binary_heaps.md).)

## Use Cases
- Dijkstra's shortest path, Prim's MST, A* pathfinding
- Huffman coding (combine the two lowest frequencies)
- Task / CPU scheduling by priority, ER triage
- Merge k sorted lists: keep a min-heap of the k heads → O(n log k) instead of O(nk)
- Top-k elements, running median
