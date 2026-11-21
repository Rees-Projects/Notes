# List (ADT)

> **Layer:** ADT | **Rule:** ordered by position | **Implemented by:** [Dynamic array](../03_data_structures/02_arrays_and_dynamic_arrays.md), [Linked list](../03_data_structures/03_linked_lists.md), static array

## Behavior
An **ordered sequence** of elements, where each element has a position (index). Duplicates are allowed.

**Core operations:**
- `get(i)` / `set(i, x)` - read or replace the item at position i
- `insert(i, x)` - add at position i (items after it move back one)
- `remove(i)` - remove at position i
- `append(x)` - add at the end
- `size()`, `is_empty()`, `contains(x)` / `index_of(x)`

## Careful: "list" has two meanings
- **List ADT** = this behavior (any language, any implementation)
- **Python `list`** = a specific **data structure** (a dynamic array) that implements the List ADT

## Implementations Compared

| Operation | Dynamic array (Python `list`) | Singly linked list (head + tail) | Doubly linked list |
|---|---|---|---|
| get(i) / set(i) | **O(1)** | O(n) | O(n) |
| append (end) | **O(1) amortized** | O(1) | O(1) |
| insert at front | O(n) - shifts | **O(1)** | **O(1)** |
| remove at front | O(n) - shifts | **O(1)** | **O(1)** |
| remove at end | O(1) | O(n) - must find prev | O(1) |
| insert/remove middle | O(n) shift | O(n) find + O(1) link | O(n) find + O(1) link |
| contains(x) | O(n) | O(n) | O(n) |
| Memory | Compact (+ spare capacity) | +1 pointer per item | +2 pointers per item |
| Cache locality | Good | Poor | Poor |

## When to Choose Which
- **Dynamic array** (the default choice): you read by index, add mostly at the end, or iterate a lot.
- **Linked list**: lots of adds/removes at the **front** (or at a node you already hold a reference to), and you rarely need index access.
- **Static array**: the size is known and fixed, memory is tight, or the language requires it (Java `int[]`).

## Used to build...
- **Stack**: a dynamic array using the end ([Stack](03_stack.md))
- **Queue**: a linked list or circular array ([Queue](04_queue_and_deque.md))
- **Adjacency list** for graphs: one list of neighbors per vertex
- **Separate chaining** in hash tables: one list per bucket
