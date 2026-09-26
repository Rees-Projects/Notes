# Data Structures Overview

## What is a Data Structure?
A data structure is a **way of organizing, storing, and managing data** so it can be accessed and modified efficiently.

Two main categories:
- **Linear** - Data arranged sequentially (Arrays, Linked Lists, Stacks, Queues)
- **Non-linear** - Data arranged hierarchically or in relationships (Trees, Graphs, Heaps)
- **Hash-based** - Key-value mappings (Hash Tables/Dictionaries)

## Why Data Structures Matter
Choice affects **time and space complexity** of operations (search, insert, delete, update). Right structure = efficient algorithm.

## Core Data Structures

| Data Structure | Organization | Access | Insert/Delete | Use Cases |
|---|---|---|---|---|
| **Array** | Contiguous memory | O(1) index | O(n) middle | Random access, fixed size |
| **Linked List** | Nodes + pointers | O(n) | O(1) head/tail* | Dynamic size, frequent inserts |
| **Stack (LIFO)** | Top-based | Top only | O(1) push/pop | Undo, recursion, expressions |
| **Queue (FIFO)** | Front/Rear | Front only | O(1) enqueue/dequeue | Scheduling, BFS, buffers |
| **Hash Table/Dict** | Key-value (hash) | O(1) avg | O(1) avg | Lookups, caching, sets |
| **Tree (BST)** | Hierarchical | O(h) | O(h) | Sorted data, search |
| **Heap (Priority Queue)** | Binary heap | O(1) max/min | O(log n) | Priority, Dijkstra's |
| **Graph** | Nodes + edges | Varies | Varies | Networks, paths, maps |

*h = height of tree, V = vertices, E = edges

## Abstract Data Types (ADTs)
An ADT defines **what operations** are possible, not **how implemented**.
- Stack ADT: push, pop, peek, is_empty
- Queue ADT: enqueue, dequeue, front, is_empty
- List ADT: get, set, insert, remove, size

## Singly-linked list: 
```python
    def append(self, item):
        self.append_node(SinglyLinkedNode(item))
# The top will get you an int input and check if it has a value or not
    def append_node(self, new_node):
        if self.head is None:
            self.head = new_node
            self.tail = new_node
        else:
            self.tail.next = new_node
            self.tail = new_node

```


## Python vs Java Notes

**Python:**
- Lists = dynamic arrays (similar to ArrayList in Java)
- Dict = hash table (HashMap)
- Set = hash set (HashSet)
- collections.deque for efficient queue/stack
- heapq for min-heap

**Java:**
- Arrays fixed, ArrayList dynamic
- LinkedList, Stack, ArrayDeque (Queue), HashMap, HashSet, PriorityQueue (heap)

Same concepts, different implementations.

## Tradeoffs
- Time vs Space
- Simplicity vs Efficiency
- Flexibility vs Performance
