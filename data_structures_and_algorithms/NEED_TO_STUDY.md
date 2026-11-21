# Need to Study

Topics I'm still learning. Each one has an explanation and self-check questions, with space at the end for my own notes.

---

## 1. List vs Array - and How Stacks & Queues Use Them

### The core confusion: "list" means two different things

| When people say... | They mean... | Layer |
|---|---|---|
| "List" (in DSA / C949) | The **List ADT**: an ordered sequence you can get, insert, and remove from by position | **ADT** (behavior, the *what*) |
| "list" (in Python) | Python's built-in `list` type, which is a **dynamic array** under the hood | **Data structure** (implementation, the *how*) |

So Python's `list` is **one implementation** of the List ADT. A linked list is another implementation of the same ADT.

### The three layers

```
 ADT (behavior / contract)        List            Stack             Queue
        "what"                      |               |                 |
                                    v               v                 v
 Data structure                Array / Dynamic   Array or         Circular array
   (implementation)            Array / Linked    Linked list      or Linked list
        "how"                  list
                                    |
                                    v
 Memory layout                 Contiguous block   vs   Nodes scattered + pointers
```

- **ADT** = the rules ("LIFO", "FIFO", "ordered by position"). It says nothing about memory.
- **Data structure** = a concrete way to store the data so those rules work.
- **Memory layout** = where the bytes actually live.

### Array

A **fixed-size, contiguous** block of memory. Every slot is the same size and sits right next to the others.

```
index:    0    1    2    3    4
        +----+----+----+----+----+
        | 10 | 20 | 30 | 40 | -- |    <- one continuous block
        +----+----+----+----+----+
```

- **O(1) access by index:** address = start + (index × slot size). This is plain arithmetic, so no searching is needed.
- **Fixed size:** you choose the capacity when you create it (Java `int[] a = new int[5];`). It can't grow.
- **Insert/delete in the middle is O(n):** every element after that point has to shift over.
- **Cache-friendly:** neighboring elements are loaded into the CPU cache together.

### Dynamic array (= Python `list`, Java `ArrayList`)

An array that **resizes itself automatically**. When it's full, it:
1. allocates a new, bigger array (usually about 2× the size)
2. copies every element over (O(n))
3. throws away the old array

```
capacity 4, full:   [10][20][30][40]
append(50)  -->     new capacity 8: [10][20][30][40][50][  ][  ][  ]
```

**Why is append still "O(1) amortized"?** The expensive O(n) copy happens rarely, and each time it happens the capacity doubles. Spread across all n appends, the total copying work is about 2n, which averages out to O(1) per append. "Amortized" means averaged over many operations, not guaranteed for every single one.

### Linked list (the other way to implement a List)

Nodes scattered anywhere in memory, each one holding a value and a pointer to the next node.

```
head -> [10 | •]--> [20 | •]--> [30 | •]--> None
```

- No resizing, ever: you just create a new node.
- O(1) insert/remove at the head (and O(1) insert at the tail if you keep a tail pointer).
- **O(n) access by index:** you have to walk from the head.
- Extra memory for every pointer, and poor cache locality.

### Array vs Dynamic Array vs Linked List

| | Array (static) | Dynamic array (Python `list`) | Linked list |
|---|---|---|---|
| Size | Fixed | Grows automatically | Grows one node at a time |
| Memory | Contiguous | Contiguous (with spare capacity) | Scattered nodes + pointers |
| Access `[i]` | O(1) | O(1) | O(n) |
| Add/remove at **end** | O(1) if room, else impossible | O(1) amortized | O(1) with tail pointer (remove tail O(n) singly) |
| Add/remove at **front** | O(n) shift | O(n) shift | **O(1)** |
| Add/remove in **middle** | O(n) | O(n) | O(n) to find + O(1) to link |
| Extra memory | None | Unused spare capacity | One pointer (or two) per element |

---

### How this connects to Stacks and Queues

**Stack and Queue are ADTs.** They only describe behavior. To actually use one, you have to **pick a data structure** to build it on, and the right choice is the one that makes the required operations O(1).

#### Stack (LIFO): only touches ONE end

| Built on... | push | pop | Verdict |
|---|---|---|---|
| Dynamic array, using the **end** | `append` O(1) amortized | `pop()` O(1) | ✅ Best choice in Python (`list`) |
| Dynamic array, using the **front** | `insert(0, x)` O(n) | `pop(0)` O(n) | ❌ Wrong end: every op shifts everything |
| Linked list, using the **head** | O(1) | O(1) | ✅ Also fine, but more memory per item |
| Static array | O(1) | O(1) | ✅ but can overflow when full |

```python
# Stack on a dynamic array (Python list) - use the END
stack = []
stack.append(1)   # push
stack.append(2)
stack.pop()       # -> 2

# Same Stack ADT, different data structure: linked list - use the HEAD
class Node:
    def __init__(self, val, next=None):
        self.val, self.next = val, next

class LinkedStack:
    def __init__(self):
        self.head = None
    def push(self, v):
        self.head = Node(v, self.head)   # new node becomes head, O(1)
    def pop(self):
        if self.head is None: return None
        v = self.head.val
        self.head = self.head.next       # O(1)
        return v
```

Both classes behave **exactly the same** from the outside (same ADT). Only the inside is different (different data structure).

#### Queue (FIFO): touches BOTH ends

A queue adds at the rear and removes from the front, so you need O(1) at **both** ends.

| Built on... | enqueue (rear) | dequeue (front) | Verdict |
|---|---|---|---|
| Python `list` | `append` O(1) | `pop(0)` **O(n)** | ❌ Front removal shifts everything |
| Circular array | O(1) | O(1) | ✅ Front/rear indexes wrap around, nothing shifts |
| Linked list (head + tail) | O(1) at tail | O(1) at head | ✅ |
| `collections.deque` | `append` O(1) | `popleft` O(1) | ✅ Best choice in Python |

**This is why a `list` works for a stack but not for a queue.** A stack only uses the cheap end. A queue also needs the front, which is the expensive end of an array.

#### Other ADTs and what they're built on

| ADT | Usually built on | Why |
|---|---|---|
| List | Dynamic array (or linked list) | Fast index access (or fast front inserts) |
| Stack | Dynamic array | One end only, and the end of an array is cheap |
| Queue / Deque | Circular array or linked list (`deque`) | Needs both ends cheap |
| Priority Queue | **Array** arranged as a binary heap | Parent/child found by index math: `2i+1`, `2i+2`, `(i-1)//2` |
| Map / Set | **Array** of buckets (hash table) | Hash → index → O(1) jump into the array |
| Graph | Dict of lists (adjacency list) | Each vertex maps to a list of its neighbors |

**Big takeaway:** arrays are the base layer under almost everything (heaps, hash tables, stacks, dynamic arrays). The O(1) index access is why so much is built on them.

### Language cheat sheet

| Concept | Python | Java |
|---|---|---|
| Static array | (no built-in; `array.array` is typed but still resizable) | `int[] a = new int[5];` |
| Dynamic array | `list` | `ArrayList<Integer>` |
| Linked list | write your own | `LinkedList<Integer>` |
| Stack | `list` (append/pop) | `ArrayDeque` (push/pop) |
| Queue | `collections.deque` | `ArrayDeque` / `LinkedList` |

### Self-check questions
1. Is a Python `list` an ADT or a data structure? Which ADT does it implement?
2. Why is `arr[5]` O(1) on an array but O(n) on a linked list?
3. What happens in memory when a dynamic array is full and you append?
4. What does "amortized O(1)" mean?
5. Why is `list.pop()` fine for a stack, but `list.pop(0)` bad for a queue?
6. Name two different data structures that can implement the Stack ADT.
7. A heap and a hash table are both built on top of which basic structure?

<details>
<summary>Answers</summary>

1. A data structure (a dynamic array). It implements the List ADT, and it can also act as a Stack.
2. An array finds the address by arithmetic (start + i × size). A linked list has to follow pointers one node at a time.
3. It allocates a bigger array (about 2×), copies every element over (O(n)), and frees the old array.
4. The cost averaged over many operations. Most appends are O(1), and the rare O(n) resize averages out.
5. `pop()` removes from the end, so nothing shifts: O(1). `pop(0)` removes from the front, so every element shifts left: O(n).
6. A dynamic array (Python `list`) and a linked list. A static array also works if the size is bounded.
7. An array.
</details>

### My notes
-

---

<!-- Add the next topic below this line -->
