# Tradeoffs & Choosing the ADT + Data Structure

Choose in two steps:
1. **Pick the ADT** from the **behavior** the problem needs (the access pattern).
2. **Pick the data structure** from **which operations happen most** and what each one costs.

---

## Step 1 - Pick the ADT: "How will I get data back out?"

```
How do I need to get items back?
│
├─ Most recent first ............................ STACK
├─ Oldest first (arrival order) ................. QUEUE
├─ From either end .............................. DEQUE
├─ Best / smallest / largest first .............. PRIORITY QUEUE
├─ By a key (name, id) .......................... MAP
├─ "Is it in there?" (unique items) ............. SET
├─ By position / index, in order ................ LIST
└─ Following connections between items .......... GRAPH (hierarchy only → TREE)
```

### Test keyword → ADT

| If the question says... | Think |
|---|---|
| "undo", "back button", "most recent", "reverse", "matching brackets", "backtrack", "nested" | **Stack** |
| "in order received", "first come first served", "waiting line", "buffer", "level by level", "BFS" | **Queue** |
| "both ends", "sliding window", "palindrome check" | **Deque** |
| "priority", "most urgent", "smallest/largest next", "top k", "schedule", "shortest path (weighted)" | **Priority Queue** |
| "look up by", "key-value", "count occurrences", "cache", "phone book", "index by id" | **Map** |
| "unique", "no duplicates", "already seen", "membership" | **Set** |
| "sorted order while inserting", "range", "next highest", "min and max" | **Sorted Map/Set (BST)** |
| "i-th element", "sequence", "position" | **List** |
| "network", "connections", "routes", "dependencies", "friends of friends" | **Graph** |

---

## Step 2 - Pick the Data Structure: "Which operations are hot?"

### List ADT

| Choose... | When... | Tradeoff |
|---|---|---|
| **Dynamic array** ★ | Index access, appending at the end, lots of iteration | Front/middle insert is O(n). The occasional resize copies O(n) |
| **Linked list** | Many inserts/removes at the **front**, or at a node you already hold | O(n) index access, extra pointer memory, slower in practice (cache) |
| **Static array** | Size is fixed and known, memory is tight | Can't grow |

### Stack ADT

| Choose... | When... | Tradeoff |
|---|---|---|
| **Dynamic array (end)** ★ | Almost always | An occasional O(n) resize (amortized O(1)) |
| **Linked list (head)** | You need a true O(1) every time (no resize spikes) | More memory per element |
| **Static array** | Max depth is known (embedded, fixed buffers) | Overflow when full |

### Queue / Deque ADT

| Choose... | When... | Tradeoff |
|---|---|---|
| **Circular array / `deque`** ★ | Almost always | A fixed-size circular array must resize or reject when full |
| **Linked list (head + tail)** | Unbounded growth, no resizing spikes | Pointer overhead |
| ~~Plain dynamic array~~ | Never for FIFO | `pop(0)` is O(n) |

### Map / Set ADT

| Choose... | When... | Tradeoff |
|---|---|---|
| **Hash table** ★ | Fast lookup/insert/delete, order doesn't matter | No sorted order. O(n) worst case on bad hashing/collisions. Keys must be hashable. Extra memory (load factor) |
| **Balanced BST** | Need sorted order, min/max, ranges, or a **guaranteed** worst case | O(log n) instead of O(1). Keys must be comparable |
| **Sorted array** | Build once, search many times | Insert/delete O(n) |

### Priority Queue ADT

| Choose... | When... | Tradeoff |
|---|---|---|
| **Binary heap** ★ | Mixed inserts and extracts | Only the min/max is fast. Searching for other items is O(n), and it isn't sorted |
| **Sorted array** | Few inserts, many extracts | Insert O(n) |
| **Unsorted array** | Many inserts, very few extracts | Extract O(n) |
| **Balanced BST** | Also need to delete arbitrary items or iterate in order | Heavier, more memory |

### Graph ADT

| Choose... | When... | Tradeoff |
|---|---|---|
| **Adjacency list** ★ | **Sparse** graphs (E much less than V²), traversals (BFS/DFS) | Checking whether edge (u,v) exists is O(degree) |
| **Adjacency matrix** | **Dense** graphs, many "is there an edge?" checks | O(V²) memory even with few edges. Listing neighbors is O(V) |
| **Edge list** | Algorithms that process edges in sorted order (Kruskal's MST) | Slow neighbor lookup |

---

## Classic Tradeoff Pairs (exam favorites)

| A vs B | Pick A when... | Pick B when... | The core tradeoff |
|---|---|---|---|
| **Array vs Linked list** | Index access, iteration, memory efficiency | Frequent front inserts/deletes | O(1) index access vs O(1) splicing |
| **Hash table vs BST** | Speed, no ordering needed | Sorted order / ranges / guaranteed worst case | O(1) avg vs O(log n) guaranteed + order |
| **Heap vs Sorted array** (for a PQ) | Mixed inserts and extracts | Static data, extract-only | O(log n) both vs O(n) insert / O(1) extract |
| **Adjacency list vs Matrix** | Sparse | Dense / O(1) edge checks | O(V+E) space vs O(V²) space |
| **Chaining vs Open addressing** (hash tables) | High load, simple deletes | Memory and cache efficiency | Extra lists vs clustering + tricky deletes |
| **`list` vs `deque`** (queue) | - (only for a stack) | Any FIFO | `pop(0)` O(n) vs `popleft` O(1) |
| **Recursion vs Iteration** | Naturally recursive (trees, divide & conquer) | Deep inputs, tight memory | Clarity vs O(n) call-stack space and stack overflow |
| **Merge sort vs Quick sort** | Need stability or a guaranteed O(n log n) | Average speed, in place | O(n) extra space vs O(n²) worst case |
| **Linear vs Binary search** | Unsorted or tiny data | Sorted data with index access | O(n) with no precondition vs O(log n) but must be sorted |

### The universal tradeoffs
To turn any of these into a justification a team will accept, frame it with the [Cost - Time - Quality triangle](03_why_it_matters_cost_time_quality.md).

- **Time ↔ Space:** caching, hash tables, and memoization buy speed with memory.
- **Average ↔ Worst case:** a hash table is fast on average. A BST, heap, or merge sort gives a guaranteed bound.
- **Ordered ↔ Fast:** keeping data sorted costs O(log n) or O(n) per update.
- **Simple ↔ Optimal:** for small n, the simple choice (a list) is often best.

---

## Practice Scenarios
Answer each one with **ADT → Data structure → Big O → Why**.

1. A web server handles requests in the order they arrive.
2. A browser's back button.
3. Check whether a username is already taken, among 10 million users.
4. An emergency room always sees the most critical patient next.
5. A leaderboard that must show players in rank order as scores change.
6. Store road connections between 50,000 cities, where each city connects to about 4 others.
7. Store a small, fixed set of 12 months by number, accessed by index.
8. Find the shortest number of hops between two people in a social network.

<details>
<summary>Answers</summary>

1. **Queue** → `deque` / circular array → O(1) enqueue/dequeue → FIFO. A `list` would make dequeue O(n).
2. **Stack** → dynamic array → O(1) push/pop → the most recent page comes back first.
3. **Set** → hash table → O(1) average contains → fastest membership check, and order is irrelevant. (A list would be O(n) per check.)
4. **Priority Queue** → binary heap → O(log n) insert/extract, O(1) peek → both operations stay fast as patients come and go.
5. **Sorted Map/Set** → balanced BST → O(log n) update, O(n) in-order listing → a hash table can't keep order, and a sorted array is O(n) per update.
6. **Graph** → adjacency list → O(V + E) space → the graph is sparse. A matrix would need 50,000² = 2.5 billion cells.
7. **List** → static array → O(1) index → the size is fixed and known, so there's no need to grow.
8. **Graph + Queue + Set** → adjacency list, `deque`, hash set → BFS in O(V + E) → BFS finds the shortest path in an unweighted graph. The set makes visited checks O(1).
</details>
