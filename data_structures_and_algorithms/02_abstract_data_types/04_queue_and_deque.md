# Queue & Deque (ADTs)

> **Layer:** ADT | **Rule:** FIFO (Queue), both ends (Deque) | **Implemented by:** circular array, [Linked list](../03_data_structures/03_linked_lists.md) (head + tail), Python `collections.deque`

## Queue - FIFO (First In, First Out)
Like a line. Enqueue at rear/back, dequeue from front. First added served first.

**Core Operations (O(1)):**
- **enqueue(x)/add** - Add to rear/back
- **dequeue()** - Remove & return front
- **front()/peek()** - View front
- **is_empty()**, **size()**

## Queue Use Cases
- CPU/job scheduling, print queues
- Breadth-First Search (BFS) - see [Trees](../03_data_structures/06_trees.md) and [Graphs](../03_data_structures/07_graphs.md)
- Buffers (streaming, I/O)
- Message queues, call centers

## Implementations Compared
A queue touches **both ends**: it adds at the rear and removes from the front. Both need to be O(1).

| Data structure | enqueue | dequeue | Tradeoff |
|---|---|---|---|
| **Python `collections.deque`** | O(1) | O(1) | ✅ Default in Python |
| **Circular array** (front index wraps) | O(1) | O(1) | ✅ Compact and cache-friendly. Fixed capacity unless it resizes |
| **Linked list** (dequeue at head, enqueue at tail) | O(1) | O(1) | ✅ Grows without limit, but extra memory per node |
| Plain Python `list` | O(1) `append` | **O(n)** `pop(0)` | ❌ Removing from the front shifts everything |

**Why a `list` works for a Stack but not a Queue:** a stack only uses the cheap end of an array. A queue also needs the front, which is the expensive end.

## Python Queue Implementation

**Using collections.deque (efficient, O(1) both ends):**
```python
from collections import deque
q = deque()
q.append(10)      # enqueue (rear) O(1)
q.append(20)
front = q[0]      # peek front O(1)
val = q.popleft() # dequeue (front) O(1) - list pop(0) is O(n)!
```

**Avoid list for front dequeue**: `list.pop(0)` is O(n) (shifts). Use `deque.popleft()`.

**Using queue.Queue (thread-safe):**
```python
from queue import Queue
q = Queue()
q.put(10)   # enqueue
q.get()     # dequeue
q.empty()
```

**Custom Queue:**
```python
from collections import deque
class Queue:
    def __init__(self):
        self.d = deque()
    def enqueue(self, v): self.d.append(v)
    def dequeue(self): return self.d.popleft() if self.d else None
    def front(self): return self.d[0] if self.d else None
    def is_empty(self): return len(self.d) == 0
    def size(self): return len(self.d)
```

## Circular Queue
Fixed-size array where the rear index wraps around to the front (`index % capacity`). Reuses freed slots at the front, so no element shifting is needed - O(1) enqueue/dequeue on a plain array.

```python
class CircularQueue:
    def __init__(self, capacity):
        self.data = [None] * capacity
        self.capacity = capacity
        self.front = 0
        self.count = 0

    def enqueue(self, v):
        if self.count == self.capacity:
            raise OverflowError("Queue full")
        rear = (self.front + self.count) % self.capacity
        self.data[rear] = v
        self.count += 1

    def dequeue(self):
        if self.count == 0:
            return None
        v = self.data[self.front]
        self.data[self.front] = None
        self.front = (self.front + 1) % self.capacity
        self.count -= 1
        return v
```

## Deque (Double-Ended Queue)
Can add/remove from both ends. Generalizes stack + queue.

```python
from collections import deque
dq = deque()
dq.append(1)       # right
dq.appendleft(0)   # left
dq.pop()           # right
dq.popleft()       # left
```

## Priority Queue
Despite the name, it's a separate ADT: items are served by **priority**, not arrival order. See [Priority Queue](06_priority_queue.md).

## Java Comparison
- Queue: `ArrayDeque<Integer>` implements Queue, or `LinkedList`
- Deque: `ArrayDeque`, `LinkedList`
- Priority queue: `PriorityQueue`

## Key Takeaways
- Queue = FIFO (front+rear). BFS, scheduling.
- Use `collections.deque` in Python for O(1) front ops.
- Circular queue = array-based queue without shifting.
