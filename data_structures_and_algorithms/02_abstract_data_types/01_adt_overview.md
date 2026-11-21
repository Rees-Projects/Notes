# Abstract Data Types (ADTs) - Overview

> **Layer:** ADT (behavior, the *what*). For the implementations (the *how*), see [Data Structures](../03_data_structures/01_data_structures_overview.md). For how the two connect, see the [Relationship Map](../05_big_picture/01_adt_ds_relationship_map.md).

## What is an ADT?
An **Abstract Data Type** describes **what** a collection does: its operations and its rules. It says **nothing** about how the data is stored.

- **ADT = contract / interface / behavior** → "a Stack gives back the most recently added item first"
- **Data structure = implementation** → "store it in a dynamic array and push/pop at the end"

**Analogy:** An ADT is like the job description "driver: gets you from A to B." The data structure is who actually does the job, a car or a bus or a bike. Same job, different costs.

**How to tell them apart on a test:**
- Does the name describe **behavior or rules** (LIFO, FIFO, unique, by key, by priority)? → **ADT**
- Does the name describe **how memory is laid out** (contiguous, nodes + pointers, buckets, array-as-tree)? → **Data structure**

| ADT (what) | ✅ | Data structure (how) | ✅ |
|---|---|---|---|
| List | ADT | Array / Dynamic array | DS |
| Stack | ADT | Linked list | DS |
| Queue / Deque | ADT | Hash table | DS |
| Set | ADT | Binary heap | DS |
| Map / Dictionary | ADT | Binary search tree (AVL, Red-Black) | DS |
| Priority Queue | ADT | Adjacency list / matrix | DS |
| Bag | ADT | Circular array | DS |

**Tree and Graph** are both: they are ADTs (hierarchy / vertices + edges) **and** families of data structures (a BST, an adjacency list). Their notes live in the data structures section because exams mostly ask about their representations.

## ADT Table

| ADT | Rule / Behavior | Core Operations | Common Implementations (Data Structures) | Notes |
|---|---|---|---|---|
| [**List**](02_list.md) | Ordered by **position** | get(i), set(i), insert(i,x), remove(i), append, size | Dynamic array, Linked list, Static array | |
| **Bag (Multiset)** | Unordered, duplicates allowed | add, remove, contains, count, size | Dynamic array, Linked list, Hash table (counts) | Python `collections.Counter` |
| [**Stack**](03_stack.md) | **LIFO**: last in, first out | push, pop, peek, is_empty, size | Dynamic array, Linked list | |
| [**Queue**](04_queue_and_deque.md) | **FIFO**: first in, first out | enqueue, dequeue, front, is_empty, size | Circular array, Linked list (head + tail) | |
| [**Deque**](04_queue_and_deque.md#deque-double-ended-queue) | Add/remove at **both** ends | push_front, push_back, pop_front, pop_back | Circular array, Doubly linked list | |
| [**Set**](05_set_and_map.md) | **Unique** items, membership | add, remove, contains, union, intersection | Hash table (hash set), Balanced BST (tree set) | |
| [**Map / Dictionary**](05_set_and_map.md) | **Key → value**, unique keys | put, get, remove, contains_key | Hash table (hash map), Balanced BST (tree map) | |
| [**Priority Queue**](06_priority_queue.md) | Highest **priority** out first | insert, extract_min/max, peek | Binary heap, (sorted/unsorted array, BST) | |
| **Tree** | Hierarchy: parent → children | root, children, insert, delete, traverse | Node + pointers, array (for complete trees) | [Trees](../03_data_structures/06_trees.md) |
| **Graph** | Vertices + edges | add_vertex, add_edge, neighbors, traverse | Adjacency list, Adjacency matrix, Edge list | [Graphs](../03_data_structures/07_graphs.md) |

## Why ADTs Matter (Engineering)
- **Swap implementations without changing the calling code.** Code that only uses `push`/`pop` doesn't care whether the stack is an array or a linked list.
- **Reason about correctness first** (what behavior the algorithm needs), then **efficiency** (which data structure makes those operations cheapest).
- In the [engineer's thinking framework](../README.md#thinking-like-an-engineer): Problem → Algorithm → **ADT** → **Data structure** → Operations → Big O → Tradeoffs.

## Self-Check
1. Is a hash table an ADT or a data structure? Which ADTs does it implement?
2. Is "Priority Queue" an ADT or a data structure? What usually implements it?
3. Name two data structures that implement the Map ADT, and give one reason to pick each.

<details>
<summary>Answers</summary>

1. A data structure. It implements Map and Set (and Bag, by storing counts).
2. An ADT. A binary heap usually implements it.
3. A hash table, for O(1) average lookup. A balanced BST, for keys kept in sorted order with O(log n) guaranteed.
</details>
