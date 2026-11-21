# Pseudocode Practice Problems

Exam-style problems, easiest first. **Use a trace table on paper** for every one, even if it looks easy. The goal is to build the habit, not just get the answer.

Every answer has been checked by running an equivalent program.

---

## Part A - Trace It (what's the output?)

**A1.**
```
x = 5
y = 2
while (x > 0) {
   x = x - y
   y = y + 1
}
print x, y
```

**A2.**
```
total = 0
for (i = 1; i <= 4; ++i) {
   total = total + i * i
}
print total
```

**A3.**
```
Mystery(numbers, numbersSize) {
   count = 0
   for (i = 0; i < numbersSize; ++i) {
      if (numbers[i] % 2 == 0) {
         count = count + 1
      }
   }
   return count
}
```
What does `Mystery([4, 7, 10, 3, 8], 5)` return? In plain words, what does it do?

**A4.**
```
G(n) {
   count = 0
   i = n
   while (i > 0) {
      i = i / 2      // integer division
      count = count + 1
   }
   return count
}
```
What does `G(20)` return?

**A5.** (recursion)
```
F(n) {
   if (n <= 1) {
      return 1
   }
   return n + F(n - 2)
}
```
What does `F(7)` return?

**A6.** (recursion order)
```
P(n) {
   if (n == 0) {
      return
   }
   print n
   P(n - 1)
}
```
What does `P(3)` print? What if `print n` were moved below `P(n - 1)`?

<details>
<summary>Part A answers</summary>

**A1.** `0 4`

| x | y | x > 0? |
|---|---|---|
| 5 | 2 | yes |
| 3 | 3 | yes |
| 0 | 4 | no, stop |

**A2.** `30` (1 + 4 + 9 + 16)

**A3.** `3` (4, 10, 8). It **counts the even numbers** (counter pattern).

**A4.** `5`. i goes 20 → 10 → 5 → 2 → 1 → 0, which is 5 halvings. This is O(log n).

**A5.** `16`. F(7) = 7 + F(5) = 7 + 5 + F(3) = 7 + 5 + 3 + F(1) = 7 + 5 + 3 + 1.

**A6.** `3 2 1`. Moving the print below the call gives `1 2 3`.
</details>

---

## Part B - Trace an Algorithm

**B1.** Binary search for `key = 23` in
`[2, 5, 8, 12, 16, 23, 38, 56, 72, 91]` (size 10), using the [binary search pseudocode](02_algorithm_fingerprints.md#binary-search). List every value of `mid`. What is returned?

**B2.** Show the array after **one** outer pass of bubble sort on `[5, 1, 4, 2, 8]`.

**B3.** Show the array after **two** outer passes of selection sort on `[29, 10, 14, 37, 13]`.

**B4.** Show the array after the outer loop of insertion sort has run for `i = 1` and `i = 2` on `[8, 3, 5, 1, 9]`.

**B5.** Merge `[2, 6, 9]` and `[1, 6, 7]` using the merge step (`if left <= right take left`). What's the result, and which `6` comes first?

**B6.** Using the zyBooks quicksort midpoint rule `midpoint = lowIndex + (highIndex - lowIndex) / 2`, what is the pivot **value** for `[12, 4, 7, 20, 15, 3, 9]`?

<details>
<summary>Part B answers</summary>

**B1.** `mid` = **4, 7, 5**. Returns **5**.

| low | high | mid | numbers[mid] | action |
|---|---|---|---|---|
| 0 | 9 | 4 | 16 | 16 < 23 → low = 5 |
| 5 | 9 | 7 | 56 | 56 > 23 → high = 6 |
| 5 | 6 | 5 | 23 | found → return 5 |

**B2.** `[1, 4, 2, 5, 8]`
- 5 vs 1 swap → `[1, 5, 4, 2, 8]`
- 5 vs 4 swap → `[1, 4, 5, 2, 8]`
- 5 vs 2 swap → `[1, 4, 2, 5, 8]`
- 5 vs 8 no swap

The largest value (8) is at the end. It happened to be there already.

**B3.** `[10, 13, 14, 37, 29]`
- Pass 1: smallest is 10 (index 1), swap with index 0 → `[10, 29, 14, 37, 13]`
- Pass 2: smallest of `29, 14, 37, 13` is 13 (index 4), swap with index 1 → `[10, 13, 14, 37, 29]`

**B4.** `[3, 5, 8, 1, 9]`
- i = 1: 3 slides before 8 → `[3, 8, 5, 1, 9]`
- i = 2: 5 slides before 8, stops at 3 → `[3, 5, 8, 1, 9]`

**B5.** `[1, 2, 6, 6, 7, 9]`. The **left** 6 comes first because of `<=`. That's what makes merge sort **stable**.

**B6.** midpoint = 0 + (6 - 0) / 2 = 3 → pivot value is **20**.
</details>

---

## Part C - Name It and Size It

For each one: **which algorithm** (or pattern) is it, and **what's the Big O**?

**C1.**
```
Q(numbers, numbersSize) {
   for (i = 1; i < numbersSize; ++i) {
      j = i
      while (j > 0 && numbers[j] < numbers[j - 1]) {
         swap numbers[j] and numbers[j - 1]
         --j
      }
   }
}
```

**C2.**
```
R(numbers, numbersSize) {
   for (i = 0; i < numbersSize - 1; ++i) {
      m = i
      for (j = i + 1; j < numbersSize; ++j) {
         if (numbers[j] < numbers[m]) {
            m = j
         }
      }
      swap numbers[i] and numbers[m]
   }
}
```

**C3.**
```
i = 1
while (i < n) {
   i = i * 2
}
```

**C4.**
```
for (i = 0; i < n; ++i) {
   for (j = i; j < n; ++j) {
      print i, j
   }
}
```
Also: how many times does `print` run when n = 4?

**C5.**
```
for (i = 0; i < n; ++i) {
   print i
}
for (j = 0; j < n; ++j) {
   print j
}
```

**C6.**
```
for (i = 0; i < n; ++i) {
   k = n
   while (k > 1) {
      k = k / 2
   }
}
```

**C7.**
```
T(n) {
   if (n <= 1) {
      return n
   }
   return T(n - 1) + T(n - 2)
}
```

<details>
<summary>Part C answers</summary>

| # | Algorithm / pattern | Big O | Clue |
|---|---|---|---|
| C1 | **Insertion sort** | O(n²), O(n) best | Outer loop starts at 1, inner loop moves backward |
| C2 | **Selection sort** | O(n²) | Remembers an index (`m`), one swap after the inner loop |
| C3 | Doubling loop | O(log n) | `i = i * 2` |
| C4 | All pairs (triangle) | O(n²) | Nested. n = 4 → 4 + 3 + 2 + 1 = **10** prints |
| C5 | Two sequential loops | O(n) | n + n = 2n → O(n) |
| C6 | Halving loop inside a linear loop | O(n log n) | n × log n |
| C7 | **Naive Fibonacci** | O(2ⁿ) | Two recursive calls, each only slightly smaller |
</details>

---

## Part D - Complete It / Fix It

**D1.** Fill in the blank so this is a correct binary search.
```
while (high >= low) {
   mid = (high + low) / 2
   if (numbers[mid] < key) {
      __________
   }
   else if (numbers[mid] > key) {
      high = mid - 1
   }
   else {
      return mid
   }
}
```

**D2.** This should return the largest value. For which input does it give a wrong answer? How do you fix it?
```
FindMax(numbers, numbersSize) {
   max = 0
   for (i = 0; i < numbersSize; ++i) {
      if (numbers[i] > max) {
         max = numbers[i]
      }
   }
   return max
}
```

**D3.** This should return the index of `key`, or `-1`. What's wrong?
```
Find(numbers, numbersSize, key) {
   for (i = 0; i < numbersSize; ++i) {
      if (numbers[i] == key) {
         return i
      }
      else {
         return -1
      }
   }
}
```

**D4.** What's wrong with this loop?
```
for (i = 0; i <= numbersSize; ++i) {
   print numbers[i]
}
```

**D5.** What's wrong with this recursion?
```
CountDown(n) {
   print n
   CountDown(n - 1)
}
```

<details>
<summary>Part D answers</summary>

**D1.** `low = mid + 1`

**D2.** Fails when **every value is negative**, e.g. `[-5, -2, -9]` returns `0`. Fix: `max = numbers[0]` and start the loop at `i = 1`.

**D3.** The `else { return -1 }` exits on the **first** element that doesn't match, so it only ever checks index 0. Move `return -1` to **after** the loop.

**D4.** `i <= numbersSize` reads one past the end (the last valid index is `numbersSize - 1`). Use `i < numbersSize`.

**D5.** No base case, so it never stops. Add `if (n < 0) { return }` (or `n == 0`) before the print.
</details>

---

## Part E - ADT Operations Trace

**E1.** Stack: `push 1, push 2, pop, push 3, push 4, pop, pop`. What values are popped, in order? What's left?

**E2.** Queue: `enqueue 1, enqueue 2, dequeue, enqueue 3, enqueue 4, dequeue, dequeue`. What values are dequeued, in order? What's left?

**E3.** Graph (adjacency lists, neighbors visited in the order listed):
```
A: B, C
B: D
C: D, E
D: F
E: F
F: (none)
```
Give the BFS visit order from A, and the recursive DFS visit order from A.

<details>
<summary>Part E answers</summary>

**E1.** Popped: **2, 4, 3**. Left: `[1]`.

**E2.** Dequeued: **1, 2, 3**. Left: `[4]`.

**E3.** BFS: **A B C D E F** (level by level: A, then B C, then D E, then F).
DFS: **A B D F C E** (A → B → D → F, dead end, back up to A → C → D already visited → E → F already visited).
</details>

---

## Score Yourself

| Part | Skill | If you missed some... |
|---|---|---|
| A | Tracing loops & recursion | Redo with a full trace table. Review [Reading Pseudocode §3](01_reading_pseudocode.md#3-how-to-trace-the-trace-table) |
| B | Tracing real algorithms | Re-read the pseudocode in [Fingerprints](02_algorithm_fingerprints.md) and trace it on 4 numbers |
| C | Recognizing & Big O | Review the [Spot It table](02_algorithm_fingerprints.md#quick-lookup-spot-it) and [Big O patterns](01_reading_pseudocode.md#4-reading-big-o-off-pseudocode) |
| D | Off-by-one & logic bugs | Review the [Gotchas](01_reading_pseudocode.md#2-the-gotchas-where-points-are-lost) and [Common Mistakes](03_writing_pseudocode.md#3-common-mistakes-when-writing) |
| E | ADT behavior | Review [Stack](../02_abstract_data_types/03_stack.md), [Queue](../02_abstract_data_types/04_queue_and_deque.md), [Graphs](../03_data_structures/07_graphs.md) |

### My notes
-
