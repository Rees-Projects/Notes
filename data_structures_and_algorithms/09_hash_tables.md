# Hash Tables (Dictionaries/Maps)

## What is a Hash Table?
A data structure that maps **keys → values** using a **hash function**. Provides average **O(1)** lookup, insertion, deletion.

**Key components:**
- **Key** - Unique identifier
- **Value** - Data stored
- **Hash function** - Converts key to index (hash code)
- **Array (bucket array)** - Stores entries at computed indices
- **Collision handling** - When different keys hash to same index

## Hash Function Properties
- **Deterministic** - Same key → same hash
- **Uniform distribution** - Spreads keys across buckets (reduces collisions)
- **Fast to compute**
- **Minimizes collisions**

## Collision Resolution

### 1. Separate Chaining
Each bucket is a list (linked list or dynamic array) of key-value pairs. Colliding keys stored in same bucket.

- **Pros**: Easy, handles many collisions, table can grow (load factor flexible)
- **Cons**: Extra memory for pointers/lists, worst case O(n) if all in one bucket

### 2. Open Addressing
Find another empty slot on collision (probe). No extra lists.

Strategies:
- **Linear probing**: index = (h(k) + i) % m, i=0,1,2,...
- **Quadratic probing**: +i²
- **Double hashing**: use second hash function

- **Pros**: Less memory overhead, better cache locality
- **Cons**: Clustering, deletion trickier, load factor must stay lower (often < 0.7)

## Load Factor
`load_factor = n / m` where n = entries, m = buckets. High load → more collisions. Resize (rehash) when threshold exceeded (common 0.7-0.8).

Resizing: create larger array, rehash all existing keys → O(n) but amortized rare.

## Time Complexity

| Operation | Average Case | Worst Case (all collide) |
|---|---|---|
| Search (get) | O(1) | O(n) |
| Insert (put) | O(1) | O(n) |
| Delete (remove) | O(1) | O(n) |

Assumes good hash function + low load factor.

## Python: dict, set
Python `dict` is a hash table (highly optimized, open addressing variant). Keys must be **hashable** (immutable: int, str, tuple, frozenset).

```python
# Dictionary
d = {"name": "Alice", "age": 30}
d["city"] = "NYC"        # insert O(1) avg
d["age"] = 31            # update O(1)
age = d.get("age", 0)    # access O(1)
del d["city"]            # delete O(1)
"name" in d              # lookup O(1) avg

# Iteration
for k in d: pass
for k,v in d.items(): pass
```

```python
# Set (hash set) - unique keys, no values
s = {1,2,3}
s.add(4)        # O(1)
s.remove(1)     # O(1)
2 in s          # O(1)
```

**Hashable example:**
```python
# tuple is hashable if elements hashable
d[(1,2)] = "point"

# list NOT hashable
# d[[1,2]] = x  # TypeError
```

## Common Use Cases
- Lookup tables, caches (LRU), frequency counts
- Indexing, symbol tables
- Sets for uniqueness (dedupe)
- Maps for key-value relationships
- Anagrams, two-sum, frequency maps

## Example: Frequency Count
```python
def char_frequency(s):
    freq = {}
    for c in s:
        freq[c] = freq.get(c, 0) + 1
    return freq  # O(n) avg
```

**Two Sum (classic):**
```python
def two_sum(nums, target):
    seen = {}
    for i, num in enumerate(nums):
        need = target - num
        if need in seen:  # O(1)
            return [seen[need], i]
        seen[num] = i
    return []  # O(n)
```

## Java Comparison
- `HashMap<K,V>` - separate chaining (or tree nodes if bucket large in recent Java)
- `TreeMap` - sorted by key (red-black tree) O(log n)
- `HashSet<E>` - backed by HashMap
- `LinkedHashMap` - preserves insertion order

## Hash Table Tradeoffs
- Excellent average-case speed
- Unordered (HashMap) vs ordered (LinkedHashMap/OrderedDict)
- Memory overhead vs arrays
- Worst case bad with poor hash or many collisions
