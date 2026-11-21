# Algorithm Fingerprints (Recognize Algorithms from Pseudocode)

Exam pseudocode often comes **without a name**, or with a name like `Mystery()`. Each core algorithm has a few lines that give it away. Learn those "fingerprints" and you can name the algorithm in seconds.

The Python versions are in [Sorting](../04_algorithms/01_sorting.md) and [Searching](../04_algorithms/02_searching.md). This file shows the same algorithms in **pseudocode** so you can compare them.

---

## Quick Lookup: Spot It

| If you see... | It's probably... | Big O |
|---|---|---|
| One loop, `if (numbers[i] == key) return i`, `return -1` after the loop | **Linear search** | O(n) |
| `low`, `high`, `mid = (low + high) / 2`, `low = mid + 1` / `high = mid - 1` | **Binary search** | O(log n) |
| Nested loops comparing `numbers[j]` and `numbers[j + 1]`, then a swap | **Bubble sort** | O(n²) |
| `indexSmallest` (or `minIndex`), inner loop finds it, **one swap per outer pass** | **Selection sort** | O(n²) |
| Inner loop moves **backward** (`j--`) and swaps or shifts while `numbers[j] < numbers[j - 1]` | **Insertion sort** | O(n²), O(n) best |
| Recursive, splits at `mid`, calls itself on both halves, then `Merge(...)` | **Merge sort** | O(n log n) |
| `pivot`, a `Partition` function, recursive calls on the two sides | **Quicksort** | O(n log n) avg, O(n²) worst |
| `queue`, `enqueue` start, loop `while queue not empty`, `dequeue`, visit neighbors | **BFS** | O(V + E) |
| `stack` (or recursion) + `visited` set, go deep into a neighbor first | **DFS** | O(V + E) |
| `if (n == base) return ...` then `return ... F(n - 1)` | **Simple recursion** (factorial, sum) | O(n) |
| `return F(n - 1) + F(n - 2)` | **Naive Fibonacci** | O(2ⁿ) |

---

## Searching

### Linear search
```
LinearSearch(numbers, numbersSize, key) {
   for (i = 0; i < numbersSize; ++i) {
      if (numbers[i] == key) {
         return i
      }
   }
   return -1   // not found
}
```
**Fingerprint:** one loop, checks every element in order, returns early on a match, `-1` after the loop.
**Needs:** nothing. Works on unsorted data.

### Binary search
```
BinarySearch(numbers, numbersSize, key) {
   low = 0
   high = numbersSize - 1
   while (high >= low) {
      mid = (high + low) / 2      // integer division
      if (numbers[mid] < key) {
         low = mid + 1            // key is in the right half
      }
      else if (numbers[mid] > key) {
         high = mid - 1           // key is in the left half
      }
      else {
         return mid               // found
      }
   }
   return -1   // not found
}
```
**Fingerprint:** `low`, `high`, `mid`. Each pass throws away half of what's left.
**Needs:** **sorted** data. This is a favorite exam question.

---

## Simple Sorts (all O(n²))

The three simple sorts look alike. Tell them apart by **what the inner loop does**.

### Bubble sort - "compare neighbors, swap, repeat"
```
BubbleSort(numbers, numbersSize) {
   for (i = 0; i < numbersSize - 1; ++i) {
      for (j = 0; j < numbersSize - i - 1; ++j) {
         if (numbers[j] > numbers[j + 1]) {
            temp = numbers[j]
            numbers[j] = numbers[j + 1]
            numbers[j + 1] = temp
         }
      }
   }
}
```
**Fingerprint:** `numbers[j]` vs `numbers[j + 1]` (side-by-side neighbors). After each outer pass, the **largest** remaining value has "bubbled" to the end.

### Selection sort - "find the smallest, swap it to the front"
```
SelectionSort(numbers, numbersSize) {
   for (i = 0; i < numbersSize - 1; ++i) {
      indexSmallest = i
      for (j = i + 1; j < numbersSize; ++j) {
         if (numbers[j] < numbers[indexSmallest]) {
            indexSmallest = j
         }
      }
      temp = numbers[i]
      numbers[i] = numbers[indexSmallest]
      numbers[indexSmallest] = temp
   }
}
```
**Fingerprint:** a variable that **remembers an index** (`indexSmallest`). The swap is **outside** the inner loop, so there's only one swap per pass. After pass k, the first k items are the k smallest, in order.

### Insertion sort - "take the next item, slide it back into place"
```
InsertionSort(numbers, numbersSize) {
   for (i = 1; i < numbersSize; ++i) {
      j = i
      while (j > 0 && numbers[j] < numbers[j - 1]) {
         temp = numbers[j]
         numbers[j] = numbers[j - 1]
         numbers[j - 1] = temp
         --j
      }
   }
}
```
**Fingerprint:** outer loop starts at **1**. Inner loop goes **backward** (`--j`) and stops as soon as the item is in place. The left part is always sorted, but it's not final - later items can still be inserted into it.

| | Bubble | Selection | Insertion |
|---|---|---|---|
| Inner loop compares | Neighbors `j` and `j + 1` | `j` vs the smallest so far | Item vs the one before it, moving left |
| Swaps | Many, inside inner loop | **One per pass**, after inner loop | Many, while sliding back |
| After pass k | Largest k at the **end** | Smallest k at the **front** (final) | First k + 1 sorted **among themselves** |
| Best case | O(n) with early exit | O(n²) always | O(n) on sorted data |

---

## Divide & Conquer Sorts (O(n log n))

### Merge sort
```
MergeSort(numbers, i, k) {
   if (i < k) {
      j = (i + k) / 2              // midpoint
      MergeSort(numbers, i, j)     // sort left half
      MergeSort(numbers, j + 1, k) // sort right half
      Merge(numbers, i, j, k)      // merge the two sorted halves
   }
}
```
`Merge` walks both halves with two positions and copies the smaller value each time:
```
// inside Merge
while (leftPos <= j && rightPos <= k) {
   if (numbers[leftPos] <= numbers[rightPos]) {   // <= keeps it stable
      mergedNumbers[mergePos] = numbers[leftPos]
      ++leftPos
   }
   else {
      mergedNumbers[mergePos] = numbers[rightPos]
      ++rightPos
   }
   ++mergePos
}
// then copy any leftovers from whichever half isn't empty
```
**Fingerprint:** split at the middle, two recursive calls, **then** a merge step. Uses a temporary array (`mergedNumbers`) → O(n) extra space.

### Quicksort
```
Quicksort(numbers, lowIndex, highIndex) {
   if (lowIndex >= highIndex) {
      return
   }
   lowEndIndex = Partition(numbers, lowIndex, highIndex)
   Quicksort(numbers, lowIndex, lowEndIndex)
   Quicksort(numbers, lowEndIndex + 1, highIndex)
}
```
`Partition` picks a **pivot** (zyBooks uses the middle element: `midpoint = lowIndex + (highIndex - lowIndex) / 2`) and moves smaller values left and larger values right.

**Fingerprint:** the word `pivot` or `Partition`. The work happens **before** the recursive calls (merge sort does its work **after**).

| | Merge sort | Quicksort |
|---|---|---|
| Where's the work? | After recursion (merge) | Before recursion (partition) |
| Split | Always exactly in half | Depends on the pivot |
| Worst case | O(n log n) | O(n²) (bad pivots) |
| Extra space | O(n) | O(log n) |

---

## Graph Traversals (O(V + E))

### BFS - breadth-first (level by level)
```
BFS(graph, startV) {
   frontierQueue = new queue
   discoveredSet = new set
   enqueue startV in frontierQueue
   add startV to discoveredSet
   while (frontierQueue is not empty) {
      currentV = dequeue from frontierQueue
      visit currentV
      for each vertex adjV adjacent to currentV {
         if (adjV is not in discoveredSet) {
            enqueue adjV in frontierQueue
            add adjV to discoveredSet
         }
      }
   }
}
```
**Fingerprint:** a **queue**. Finds shortest paths in unweighted graphs.

### DFS - depth-first (go deep, then back up)
```
DFS(graph, startV) {
   vertexStack = new stack
   visitedSet = new set
   push startV to vertexStack
   while (vertexStack is not empty) {
      currentV = pop from vertexStack
      if (currentV is not in visitedSet) {
         visit currentV
         add currentV to visitedSet
         for each vertex adjV adjacent to currentV {
            push adjV to vertexStack
         }
      }
   }
}
```
**Fingerprint:** a **stack** (or the function calling itself on a neighbor). Swap the queue in BFS for a stack and you get DFS.

---

## Self-check questions
1. What one line tells you a search requires sorted data?
2. Selection vs bubble sort: where is the swap in each?
3. Which simple sort has the outer loop start at index 1?
4. Merge sort and quicksort are both recursive. How do you tell them apart?
5. BFS and DFS have almost the same pseudocode. What's the one difference?

<details>
<summary>Answers</summary>

1. `mid = (low + high) / 2` together with `low = mid + 1` / `high = mid - 1`. Throwing away half only works if the data is sorted.
2. Bubble: inside the inner loop (many swaps). Selection: after the inner loop (one swap per pass).
3. Insertion sort (index 0 alone is already "sorted").
4. Merge sort splits first and does the work (merge) after the recursive calls. Quicksort does the work (partition around a pivot) before the recursive calls.
5. BFS uses a queue (FIFO). DFS uses a stack (LIFO) or recursion.
</details>

### My notes
-
