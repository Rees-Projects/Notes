# Data Structures Overview

> **Layer:** Data structure (implementation, the *how*). For the behaviors these implement, see [ADTs](../02_abstract_data_types/01_adt_overview.md). For how the two connect, see the [Relationship Map](../05_big_picture/01_adt_ds_relationship_map.md).

## What is a Data Structure?
A data structure is a **concrete way of organizing and storing data in memory** so that certain operations are efficient. It is the **implementation** of one or more ADTs.

Three main categories:
- **Linear** - stored in sequence (Arrays, Dynamic arrays, Linked lists, Circular arrays)
- **Non-linear** - hierarchy or network (Trees, Heaps, Graph representations)
- **Hash-based** - key → hash → bucket index (Hash tables)

## The Two Building Blocks
Every data structure is built from one (or both) of these:

| Building block | Memory | Strength | Weakness |
|---|---|---|---|
| **Array** (contiguous) | One block, slots side by side | O(1) access by index (address arithmetic), cache-friendly | Fixed size; insert/remove in the middle shifts O(n) |
| **Nodes + pointers** (linked) | Scattered nodes linked by references | O(1) insert/remove where you already have a pointer, grows freely | O(n) to reach the i-th item, extra pointer memory, poor cache locality |

| Data structure | Built from |
|---|---|
| Dynamic array | Array (+ resize by copying) |
| Circular array | Array (+ wrap-around indexes) |
| Linked list | Nodes + pointers |
| Hash table | Array of buckets (+ linked lists in each bucket if using separate chaining) |
| Binary heap | Array (tree positions found by index math) |
| BST / AVL / Red-Black | Nodes + pointers |
| Adjacency list | Array/dict of lists |
| Adjacency matrix | 2D array |

## Data Structures Compared

| Data structure | Implements ADT(s) | Access / Search | Insert | Delete | Use when |
|---|---|---|---|---|---|
| [**Array (static)**](02_arrays_and_dynamic_arrays.md) | List | O(1) index / O(n) search | O(n) middle | O(n) middle | Fixed size, fastest index access |
| [**Dynamic array**](02_arrays_and_dynamic_arrays.md) | List, Stack | O(1) index / O(n) search | O(1)* end, O(n) front | O(1) end, O(n) front | Default general-purpose sequence |
| **Circular array** | Queue, Deque | O(1) ends | O(1) ends | O(1) ends | Array-based FIFO with no shifting |
| [**Linked list**](03_linked_lists.md) | List, Stack, Queue, Deque | O(n) | O(1) head (tail w/ ptr) | O(1) head** | Frequent front inserts/removes |
| [**Hash table**](04_hash_tables.md) | Map, Set, Bag | O(1) avg by key | O(1) avg | O(1) avg | Fast lookup by key, no order needed |
| [**Binary heap**](05_binary_heaps.md) | Priority Queue | O(1) min/max only | O(log n) | O(log n) extract | Repeatedly take the min/max |
| [**BST (balanced)**](06_trees.md) | Sorted Map, Sorted Set | O(log n) | O(log n) | O(log n) | Sorted order + fast search |
| [**Adjacency list / matrix**](07_graphs.md) | Graph | list: O(deg) edge check; matrix: O(1) | O(1) add edge | varies | Networks, routes, dependencies |

\*amortized. \*\*Deleting the tail is O(n) in a singly linked list (it has to find the previous node) and O(1) in a doubly linked list. An unbalanced BST degrades to O(n).

## Python vs Java

| Data structure | Python | Java |
|---|---|---|
| Static array | (none built-in; `array.array` is typed) | `int[]` |
| Dynamic array | `list` | `ArrayList` |
| Linked list | write your own | `LinkedList` |
| Circular array / deque | `collections.deque` | `ArrayDeque` |
| Hash table | `dict`, `set` | `HashMap`, `HashSet` |
| Binary heap | `heapq` (on a `list`) | `PriorityQueue` |
| Balanced BST | (none built-in) | `TreeMap`, `TreeSet` (Red-Black) |

## Tradeoffs in General
- **Time vs space:** hash tables and memoization spend memory to save time.
- **Average vs worst case:** a hash table is O(1) average but O(n) worst; a balanced BST is O(log n) guaranteed.
- **Order vs speed:** keeping data sorted (BST, sorted array) makes some operations more expensive.
- **Simplicity vs performance:** a plain list is often fast enough for small n.

Full decision guide: [Tradeoffs & Choosing ADT + DS](../05_big_picture/02_tradeoffs_decision_guide.md).
