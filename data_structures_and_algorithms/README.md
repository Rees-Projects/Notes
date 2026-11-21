# Data Structures & Algorithms - Overview (C949)

## Table of Contents

### 1. Foundations
| # | Topic | What's inside |
|---|---|---|
| 1.1 | [Algorithms Basics](01_foundations/01_algorithms_basics.md) | What an algorithm is, characteristics, ADT vs data structure, time & space complexity intro |
| 1.2 | [Problem Solving & Algorithm Analysis](01_foundations/02_problem_solving_and_algorithm_analysis.md) | I.Q.O.C.E. process, **Problem → Tradeoffs framework**, best/avg/worst case, Big O / Ω / Θ, growth rates |
| 1.3 | [Recursion](01_foundations/03_recursion.md) | Base/recursive case, call stack, factorial, Fibonacci, memoization, tail recursion |
| 1.4 | [Big O Cheatsheet](01_foundations/04_big_o_cheatsheet.md) | Quick reference: common complexities, Python operation costs, recurrences |

### 2. Abstract Data Types (behavior - the *what*)
| # | ADT | Rule | Usually implemented by |
|---|---|---|---|
| 2.1 | [ADT Overview](02_abstract_data_types/01_adt_overview.md) | ADT vs data structure, how to tell them apart, full ADT table | - |
| 2.2 | [List](02_abstract_data_types/02_list.md) | Ordered by position | Dynamic array, linked list |
| 2.3 | [Stack](02_abstract_data_types/03_stack.md) | LIFO | Dynamic array (end), linked list (head) |
| 2.4 | [Queue & Deque](02_abstract_data_types/04_queue_and_deque.md) | FIFO / both ends | Circular array (`deque`), linked list |
| 2.5 | [Set & Map](02_abstract_data_types/05_set_and_map.md) | Unique items / key → value | Hash table, balanced BST |
| 2.6 | [Priority Queue](02_abstract_data_types/06_priority_queue.md) | Highest priority first | Binary heap |

### 3. Data Structures (implementation - the *how*)
| # | Data structure | Built from | Implements ADT(s) |
|---|---|---|---|
| 3.1 | [Data Structures Overview](03_data_structures/01_data_structures_overview.md) | The two building blocks, comparison table | - |
| 3.2 | [Arrays & Dynamic Arrays](03_data_structures/02_arrays_and_dynamic_arrays.md) | Contiguous memory | List, Stack (and Queue as a circular array) |
| 3.3 | [Linked Lists](03_data_structures/03_linked_lists.md) | Nodes + pointers | List, Stack, Queue, Deque |
| 3.4 | [Hash Tables](03_data_structures/04_hash_tables.md) | Array of buckets + hash function | Map, Set |
| 3.5 | [Binary Heaps](03_data_structures/05_binary_heaps.md) | Array viewed as a tree | Priority Queue |
| 3.6 | [Trees (BST)](03_data_structures/06_trees.md) | Nodes + pointers | Sorted Map / Set (also the Tree ADT) |
| 3.7 | [Graphs](03_data_structures/07_graphs.md) | Adjacency list / matrix | Graph ADT + BFS/DFS |

### 4. Algorithms
| # | Topic | What's inside |
|---|---|---|
| 4.1 | [Sorting](04_algorithms/01_sorting.md) | Bubble, selection, insertion, merge, quick, heap, counting/radix/bucket |
| 4.2 | [Searching](04_algorithms/02_searching.md) | Linear, binary (+ variants), jump, interpolation, exponential, ternary |

### 5. Big Picture (test prep)
| # | Topic | What's inside |
|---|---|---|
| 5.1 | [ADT ↔ Data Structure Relationship Map](05_big_picture/01_adt_ds_relationship_map.md) | Layer diagram, ADT × DS matrix, "built on" chains, algorithm → ADT table |
| 5.2 | [Tradeoffs & Choosing ADT + DS](05_big_picture/02_tradeoffs_decision_guide.md) | Decision tree, test keyword → ADT table, when to choose each DS, classic tradeoff pairs, practice scenarios |
| 5.3 | [Why It Matters: Cost - Time - Quality](05_big_picture/03_why_it_matters_cost_time_quality.md) | The equilateral triangle, how DSA choices affect each side, a justification template, worked examples |

### Study List
| Topic | What's inside |
|---|---|
| [Need to Study](NEED_TO_STUDY.md) | List vs array vs dynamic array vs linked list, and how stacks / queues / other ADTs are built on them |

---

## Thinking Like an Engineer

Work through a problem in this order. Each step answers a question that the step before it raises.

| # | Step | Question it answers | Example: "Find the fewest hops between two people in a social network" |
|---|---|---|---|
| 1 | **Problem** | What exactly must be solved? (inputs, outputs, constraints) | Input: graph of friendships + 2 people. Output: shortest chain of friends |
| 2 | **Solution strategy (algorithm)** | What approach solves it? | Breadth-First Search (explores level by level, so the first time it reaches the target is the shortest path) |
| 3 | **Required behavior (ADT)** | What behavior does that strategy need from its data? | BFS needs to process people in the order discovered → **FIFO → Queue ADT** (plus a **Set** ADT for "already visited") |
| 4 | **Implementation (data structure)** | Which concrete structure provides that behavior? | Queue → `collections.deque` (not a `list` - see tradeoffs). Set → hash set (`set`) |
| 5 | **Operations** | What operations will the algorithm call? | `append` (enqueue), `popleft` (dequeue), `add` / `in` on the visited set |
| 6 | **Runtime (Big O)** | What does each operation cost, and what's the total? | Each op O(1) → BFS total **O(V + E)** time, O(V) space |
| 7 | **Why - tradeoffs** | Why this choice over the alternatives? | `list.pop(0)` is O(n) and would make BFS O(V² + E). `deque` gives O(1) at both ends. A hash set gives O(1) membership checks, vs O(n) for a list |

**Key idea:** The **ADT is the contract** (what behavior), the **data structure is the implementation** (how). Several data structures can satisfy the same ADT. The operations you need and their Big O costs decide which one wins.

**In practice it loops:** If the runtime or tradeoffs in steps 6-7 aren't good enough, go back to step 2 or 4 and pick a different strategy or structure.

**Justifying the choice (Step 7):** An engineer defends a tradeoff with the **Cost - Time - Quality triangle**. The three sides are equal and linked, so improving one moves another. "I chose a hash set: it costs more memory (**cost**), ships today because it's built in (**time**), and gives O(1) lookups at 10M users (**quality**)." Full guide: [Why It Matters](05_big_picture/03_why_it_matters_cost_time_quality.md).

### The Framework Applied

| Problem | Strategy (algorithm) | ADT | Data structure | Key operations | Big O | Why / tradeoff |
|---|---|---|---|---|---|---|
| Undo button in an editor | Reverse the most recent action first | Stack (LIFO) | Dynamic array (`list`) | push, pop | O(1) each | Only the newest item is needed, so a stack fits. `list` end ops are O(1) and have little overhead |
| Print jobs served in order | Serve first-come, first-served | Queue (FIFO) | `deque` / circular array | enqueue, dequeue | O(1) each | `list.pop(0)` would be O(n). A circular array is used when the size is fixed |
| Look up a contact by name | Direct key lookup | Map / Dictionary | Hash table (`dict`) | put, get | O(1) avg, O(n) worst | Fastest lookup, but unordered. Use a tree map if you need the names in sorted order |
| ER triage: treat most urgent first | Always take the highest priority next | Priority Queue | Binary heap (`heapq`) | insert, extract-min | O(log n) each, peek O(1) | A sorted list makes extraction O(1) but insertion O(n). A heap balances both |
| Leaderboard that stays sorted as scores change | Keep items ordered while inserting and deleting | Sorted Map / Set | Balanced BST (AVL / Red-Black) | insert, delete, in-order traversal | O(log n) each | A hash table can't keep order. A sorted array has O(log n) search but O(n) insert |
| Detect duplicate usernames | Membership test | Set | Hash set (`set`) | add, contains | O(1) avg | Checking with nested loops would be O(n²). Costs O(n) extra memory |
| Sort 1M records, equal keys keep their order | Divide & conquer | List | Array | merge | O(n log n) all cases | Merge sort is stable and predictable, at the cost of O(n) extra space. Quick sort is faster on average but unstable, with an O(n²) worst case |
| Find an item in sorted data | Halve the search space | List | Sorted array | index access | O(log n) | Requires sorted data and O(1) index access. A linked list can't do binary search efficiently |

---

## ADT → Data Structure at a Glance

The **ADT** is the behavior you need. The **data structure** is what usually provides it. Full picture: [Relationship Map](05_big_picture/01_adt_ds_relationship_map.md). When to choose what: [Tradeoffs Guide](05_big_picture/02_tradeoffs_decision_guide.md).

| ADT (what) | Rule | Usual data structure (how) | Key ops & Big O | Alternative DS & when | Python |
|---|---|---|---|---|---|
| [List](02_abstract_data_types/02_list.md) | Ordered by position | [Dynamic array](03_data_structures/02_arrays_and_dynamic_arrays.md) | get O(1), append O(1)*, insert front O(n) | [Linked list](03_data_structures/03_linked_lists.md): many front inserts | `list` |
| [Stack](02_abstract_data_types/03_stack.md) | LIFO | Dynamic array (end) | push O(1)*, pop O(1) | Linked list (head): no resize spikes | `list` |
| [Queue](02_abstract_data_types/04_queue_and_deque.md) | FIFO | Circular array | enqueue O(1), dequeue O(1) | Linked list (head + tail): unbounded | `collections.deque` |
| [Map](02_abstract_data_types/05_set_and_map.md) | Key → value | [Hash table](03_data_structures/04_hash_tables.md) | get/put O(1) avg, O(n) worst | [Balanced BST](03_data_structures/06_trees.md): sorted keys, O(log n) | `dict` |
| [Set](02_abstract_data_types/05_set_and_map.md) | Unique items | Hash table | add/contains O(1) avg | Balanced BST: sorted, O(log n) | `set` |
| [Priority Queue](02_abstract_data_types/06_priority_queue.md) | Best first | [Binary heap](03_data_structures/05_binary_heaps.md) | insert/extract O(log n), peek O(1) | Sorted array: few inserts | `heapq` |
| Graph | Vertices + edges | [Adjacency list](03_data_structures/07_graphs.md) | BFS/DFS O(V+E), O(V+E) space | Adjacency matrix: dense, O(1) edge check | `dict` of lists |

\*amortized. An unbalanced BST degrades to O(n). A hash table degrades to O(n) when all keys collide.

## Algorithms at a Glance

| Algorithm | Best | Average | Worst | Space | Stable | Needs |
|---|---|---|---|---|---|---|
| Bubble sort | O(n) | O(n²) | O(n²) | O(1) | Yes | - |
| Selection sort | O(n²) | O(n²) | O(n²) | O(1) | No | - |
| Insertion sort | O(n) | O(n²) | O(n²) | O(1) | Yes | - |
| Merge sort | O(n log n) | O(n log n) | O(n log n) | O(n) | Yes | - |
| Quick sort | O(n log n) | O(n log n) | O(n²) | O(log n) | No | Good pivot |
| Heap sort | O(n log n) | O(n log n) | O(n log n) | O(1) | No | - |
| Linear search | O(1) | O(n) | O(n) | O(1) | - | Nothing |
| Binary search | O(1) | O(log n) | O(log n) | O(1) | - | **Sorted** data |
| BFS / DFS | - | O(V+E) | O(V+E) | O(V) | - | Graph / tree |

## Where DSA Fits in the SDLC
DSA work happens inside the software development life cycle (simplified waterfall: **Plan → Analyze → Design → Implement → Test → Maintain**):

| Phase | DSA work |
|---|---|
| Plan / Analyze | Clarify input, output, constraints, and size of *n* (I.Q.O.C.E. in 1.2) |
| Design | Pick the ADT, then the data structure; write pseudocode; estimate Big O |
| Implement | Code it, using built-ins where they fit (`dict`, `deque`, `heapq`) |
| Test | Edge cases: empty, one element, duplicates, sorted / reverse-sorted input |
| Maintain | As data grows, profile and swap structures/algorithms that become bottlenecks |
