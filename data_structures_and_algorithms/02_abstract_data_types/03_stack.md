# Stack (ADT)

> **Layer:** ADT | **Rule:** LIFO | **Implemented by:** [Dynamic array](../03_data_structures/02_arrays_and_dynamic_arrays.md) (usual), [Linked list](../03_data_structures/03_linked_lists.md), static array

## Stack - LIFO (Last In, First Out)
Like a stack of plates. Push onto top, pop from top. Only top accessible.

**Core Operations (O(1)):**
- **push(x)** - Add to top
- **pop()** - Remove & return top
- **peek()/top()** - View top without removing
- **is_empty()** - Check if empty
- **size()** - Number of elements

## Stack Use Cases
- Function calls / Call stack (recursion) - see [Recursion](../01_foundations/03_recursion.md)
- Undo/Redo operations
- Expression evaluation (infix → postfix/prefix)
- Parentheses/bracket validation (balanced)
- Backtracking (DFS) - see [Graphs](../03_data_structures/07_graphs.md)
- Browser history (back button)

## Implementations Compared
A stack only ever touches **one end**. The right data structure is whichever one makes that end O(1).

| Data structure | push | pop | peek | Tradeoff |
|---|---|---|---|---|
| **Dynamic array, using the end** (Python `list`) | O(1) amortized | O(1) | O(1) | ✅ Default choice: simple, compact, cache-friendly. An occasional resize costs O(n) |
| Linked list, using the head | O(1) | O(1) | O(1) | ✅ No resizing and a true O(1) every time, but extra memory per node |
| Static array + top index | O(1) | O(1) | O(1) | Fixed capacity, so it can **overflow**. Used when memory is bounded |
| Dynamic array, using the **front** | O(n) | O(n) | O(1) | ❌ Wrong end: everything shifts |

## Python Stack Implementation

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

**Using collections.deque (also fine):**
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

**Linked-list stack (same ADT, different data structure):**
```python
class _Node:
    def __init__(self, val, next=None):
        self.val, self.next = val, next

class LinkedStack:
    def __init__(self):
        self.head = None
        self._size = 0
    def push(self, v):
        self.head = _Node(v, self.head)  # new node becomes the top
        self._size += 1
    def pop(self):
        if self.head is None: return None
        v, self.head = self.head.val, self.head.next
        self._size -= 1
        return v
    def peek(self): return self.head.val if self.head else None
    def is_empty(self): return self.head is None
    def size(self): return self._size
```

## Example: Balanced Parentheses
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

assert is_balanced("({[]})")
assert not is_balanced("(]")
assert not is_balanced("((")
assert is_balanced("")
```

## Java Comparison
- `Stack<Integer>` (legacy) or `ArrayDeque<Integer>` as stack (push/pop) - `ArrayDeque` preferred

## Key Takeaways
- Stack = LIFO (top only). DFS, undo, recursion.
- Python `list` is a fine stack (append/pop at the end are O(1)).
