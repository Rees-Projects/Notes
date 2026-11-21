# Set & Map / Dictionary (ADTs)

> **Layer:** ADT | **Implemented by:** [Hash table](../03_data_structures/04_hash_tables.md) (hash set / hash map) or [Balanced BST](../03_data_structures/06_trees.md) (tree set / tree map)

## Set - Behavior
A collection of **unique** items. The main question it answers is **"is x in here?"**

**Core operations:** `add(x)`, `remove(x)`, `contains(x)`, `size()`, plus `union`, `intersection`, `difference`

## Map / Dictionary - Behavior
Stores **key → value** pairs with **unique keys**. Looks up a value by its key.

**Core operations:** `put(k, v)`, `get(k)`, `remove(k)`, `contains_key(k)`, `keys()`, `values()`

**Relationship:** A Set is like a Map that only has keys. In Java, `HashSet` is literally built on a `HashMap`.

## Implementations Compared

| | Hash table (`dict`, `set`, `HashMap`) | Balanced BST (`TreeMap`, `TreeSet`) | Unsorted list | Sorted array |
|---|---|---|---|---|
| contains / get | **O(1) avg**, O(n) worst | O(log n) | O(n) | O(log n) (binary search) |
| add / put | **O(1) avg** | O(log n) | O(1) (if no dup check) | O(n) (shift) |
| remove | **O(1) avg** | O(log n) | O(n) | O(n) |
| Iterate in **sorted** order | ❌ O(n log n) (sort first) | ✅ O(n) (in-order) | ❌ | ✅ O(n) |
| min / max / range queries | ❌ O(n) | ✅ O(log n) | ❌ O(n) | ✅ O(1) / O(log n) |
| Key requirement | **Hashable** (immutable) | **Comparable** (orderable) | Equality | Comparable |
| Worst-case guarantee | No (collisions) | **Yes** | Yes | Yes |

## When to Choose Which
- **Hash table**: the default. Use it when you need the fastest lookup and don't care about order. Examples: counting, dedupe, caching, two-sum.
- **Balanced BST (tree map/set)**: use it when you need keys **in sorted order**, min/max, "next bigger key", range queries, or a guaranteed O(log n) worst case.
- **Sorted array**: for data that is built once and searched many times (few inserts).
- **Unsorted list**: only for tiny collections.

## Python / Java

| ADT | Python | Java (hash) | Java (tree, sorted) |
|---|---|---|---|
| Set | `set` | `HashSet` | `TreeSet` |
| Map | `dict` | `HashMap` | `TreeMap` |
| Ordered by insertion | `dict` (3.7+) | `LinkedHashMap` | - |

Python has no built-in tree map. Use `sorted()` on the keys, or `bisect` with a sorted list.

```python
seen = set()
for name in ["ana", "bo", "ana"]:
    if name in seen:          # O(1) avg
        print("duplicate:", name)
    seen.add(name)

ages = {"ana": 30}
ages["bo"] = 25               # put O(1) avg
print(ages.get("cy", 0))      # get with default
```
