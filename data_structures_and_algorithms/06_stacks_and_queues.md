# Stacks & Queues

## Stack - LIFO (Last In, First Out)
Like a stack of plates. Push onto top, pop from top. Only top accessible.

**Core Operations (O(1)):**
- **push(x)** / push - Add to top
- **pop()** - Remove & return top
- **peek()/top()** - View top without removing
- **is_empty()** - Check if empty
- **size()** - Number of elements

### Stack Use Cases
- Function calls / Call stack (recursion)
- Undo/Redo operations
- Expression evaluation (infix → postfix/prefix)
- Parentheses/bracket validation (balanced)
- Backtracking (DFS)
- Browser history (back button)

### Python Stack Implementation

**Using list (simple):**
```python
stack = []
stack.append(10)  # push O(1) amortized
stack.append(20)
top = stack[-1]   # peek O(1)
val = stack.pop() # pop O(1)
if not stack:     # empty
    pass
```

**Using collections.deque (recommended):**
```python
from collections import deque
stack = deque()
stack.append(10)
stack.append(20)
stack.pop()  # O(1)
```

**Custom Stack:**
```python
class Stack:
    def __init__(self):
        self.items = []
    def push(self, v): self.items.append(v)
    def pop(self): return self.items.pop() if self.items else None
    def peek(self): return self.items[-1] if self.items else None
    def is_empty(self): return len(self.items) == 0
    def size(self): return len(self.items)
```

### Example: Balanced Parentheses
```python
def is_balanced(s):
    pairs = {')':'(', '}':'{', ']':'['}
    st = []
    for c in s:
        if c in '({[':
            st.append(c)
        elif c in ')}]':
            if not st or st.pop() != pairs[c]:
                return False
    return len(st) == 0  # O(n)
```

## Queue - FIFO (First In, First Out)
Like a line. Enqueue at rear/back, dequeue from front. First added served first.

**Core Operations (O(1)):**
- **enqueue(x)/add** - Add to rear/back
- **dequeue()** - Remove & return front
- **front()/peek()** - View front
- **is_empty()**, **size()**

### Queue Use Cases
- CPU/job scheduling, print queues
- Breadth-First Search (BFS)
- Buffers (streaming, I/O)
- Message queues, call centers

### Python Queue Implementation

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
Fixed size, rear wraps around to front when full (efficient array-based queue, avoids front shifting).

## Deque (Double-Ended Queue)
Can enqueue/dequeue from both ends. Generalizes stack + queue.

```python
from collections import deque
dq = deque()
dq.append(1)       # right
dq.appendleft(0)   # left
dq.pop()           # right
dq.popleft()       # left
```

## Java Comparison
- Stack: `Stack<Integer>` or `ArrayDeque<Integer>` as stack (push/pop)
- Queue: `ArrayDeque<Integer>` implements Queue, or `LinkedList`, or `PriorityQueue`
- Deque: `ArrayDeque`, `LinkedList`

## Key Takeaways
- Stack = LIFO (top only). DFS, undo, recursion.
- Queue = FIFO (front+rear). BFS, scheduling.
- Use `collections.deque` in Python for O(1) front ops.
