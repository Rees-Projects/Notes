# Big O - Quick Reference & Examples

## Common Complexities (Fast → Slow Growth as n↑)

| Big O | Name | Example |
|---|---|---|
| O(1) | Constant | Array access, dict lookup (avg), push/pop end |
| O(log n) | Logarithmic | Binary search, balanced BST ops |
| O(n) | Linear | Loop once, linear search, array traversal |
| O(n log n) | Linearithmic | Merge sort, heap sort, Timsort (sorted()) |
| O(n²) | Quadratic | Nested loops, bubble/selection/insertion worst |
| O(n³) | Cubic | Triple nested loops |
| O(2ⁿ) | Exponential | Recursive Fibonacci (naive), subsets |
| O(n!) | Factorial | Permutations, TSP brute force |

## Drop Constants & Lower Order Terms
O(2n + 3) → O(n), O(n² + n log n + n) → O(n²). Focus on dominant term for large n.

## Time Complexity Examples (Python)

| Code | Complexity | Explanation |
|---|---|---|
| `arr[i]` | O(1) | Direct index |
| `if x in arr:` (list) | O(n) | Linear scan; use `set/dict` for O(1) avg |
| `x in s` (set/dict) | O(1) avg | Hash table |
| `arr.append(x)` | O(1) amortized | End of dynamic array |
| `arr.insert(0,x)` | O(n) | Shifts all elements |
| `arr.pop()` | O(1) | End |
| `arr.pop(0)` | O(n) | Shifts (use deque.popleft()) |
| `for v in arr: ...` | O(n) | Single pass |
| `for i in arr: for j in arr: ...` | O(n²) | Double nested |
| `sorted(arr)` | O(n log n) | Timsort |
| `len(arr)` | O(1) | Cached |
| `arr[::-1]` slice copy | O(n) | Creates new list |
| `"".join(arr)` | O(n) | String build |
| `binary_search(sorted_arr, t)` | O(log n) | Halving |

## Loops Cheat Sheet
- Single loop over n: O(n)
- Nested loops i,j 0..n-1: O(n²)
- Loop i from 1 doubling: `i*=2` → O(log n)
- Loop i*=2 inside outer n: O(n log n)
- Loop up to n, then inside up to n decreasing? still O(n²) in total

## Space Complexity Examples

| Pattern | Space | Notes |
|---|---|---|
| Variables only | O(1) | No extra data structures growing with n |
| Copy of array/list of size n | O(n) | `arr[:]`, `new_list = list(arr)` |
| Recursion depth n | O(n) | Call stack (factorial, deep tree skewed) |
| Hash map storing n keys | O(n) | Extra storage |
| Merge sort auxiliary arrays | O(n) | Temporary arrays during merge |
| Quick sort recursion | O(log n) avg, O(n) worst | Stack depth |

## Recurrence (Quick Mental)
- T(n)=T(n/2)+O(1) → O(log n) (binary search)
- T(n)=2*T(n/2)+O(n) → O(n log n) (merge sort)
- T(n)=2*T(n/2)+O(1) → O(n)
- T(n)=T(n-1)+O(1) → O(n) (loop/recursion down)
- T(n)=T(n-1)+O(n) → O(n²)

## Big O Rules Summary
1. **Worst case** is usually what matters
2. **Remove constants** (3n → n)
3. **Drop non-dominant terms** (n²+n → n²)
4. **Consider input size n** as variable
5. **Time != Space** - analyze both if asked
6. **Average vs Worst** - clarify if needed (hash tables O(1) avg)
