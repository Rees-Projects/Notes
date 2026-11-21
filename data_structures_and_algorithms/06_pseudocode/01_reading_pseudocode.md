# Reading Pseudocode

Pseudocode is an algorithm written for **humans**, not a compiler. There is no single official syntax. The goal is to show the **logic** (loops, decisions, data movement) without language details.

**Why this matters for C949:** many questions show a block of pseudocode and ask you to:
1. **Trace** it - what is the output / final value?
2. **Recognize** it - which algorithm is this?
3. **Analyze** it - what is its Big O?
4. **Complete / fix** it - which line fills the blank, or which line is wrong?

All four skills start with the same thing: reading it line by line without guessing. This file covers that.

---

## 1. The Vocabulary (Pseudocode → Python)

The course material (zyBooks) mostly uses a **C/Java-like style with braces**. You will also see a plain-English style. Learn to read both.

| Idea | Brace style (zyBooks-like) | English style | Python |
|---|---|---|---|
| Assign | `x = 5` | `set x to 5` / `x ← 5` | `x = 5` |
| Compare equal | `x == 5` | `x = 5` (in an `if`) | `x == 5` |
| Not equal | `x != 5` | `x ≠ 5` | `x != 5` |
| Count loop | `for (i = 0; i < n; ++i) { }` | `for i from 0 to n - 1` | `for i in range(n):` |
| Inclusive loop | `for (i = 1; i <= n; ++i) { }` | `for i from 1 to n` | `for i in range(1, n + 1):` |
| Counting down | `for (i = n - 1; i >= 0; --i)` | `for i from n - 1 down to 0` | `for i in range(n - 1, -1, -1):` |
| Condition loop | `while (low <= high) { }` | `while low ≤ high` | `while low <= high:` |
| Decision | `if (...) { } else if (...) { } else { }` | `if ... then ... else ...` | `if / elif / else` |
| Function | `FindMax(numbers, numbersSize) { }` | `procedure FindMax(A, n)` | `def find_max(numbers):` |
| Return | `return max` | `return max` | `return max` |
| Array access | `numbers[i]` | `A[i]` | `numbers[i]` |
| Array size | `numbersSize` (passed in) | `n` / `length(A)` | `len(numbers)` |
| Increment | `++i` / `i = i + 1` | `increment i` | `i += 1` |
| Remainder | `x % 2` | `x mod 2` | `x % 2` |
| Swap | `temp = a; a = b; b = temp` | `swap A[i] and A[j]` | `a, b = b, a` |

**Note:** in brace-style pseudocode, arrays usually don't know their own size, so the size is passed in as its own parameter (`numbersSize`). That's why so many functions have two parameters.

---

## 2. The Gotchas (where points are lost)

| Gotcha | What to watch for | Example |
|---|---|---|
| **Integer division** | `/` on two integers drops the decimal | `mid = (0 + 9) / 2` → `4`, not `4.5` |
| **`<` vs `<=`** | Changes how many times a loop runs | `i < 5` runs for 0-4 (5 times). `i <= 5` runs for 0-5 (6 times) |
| **0-based vs 1-based** | Check where the loop starts | `for i from 1 to n` usually means a 1-based array |
| **`=` vs `==`** | In brace style `=` assigns and `==` compares | `if (x == 0)` |
| **Order of updates** | A line uses the **new** value of a variable changed above it | `x = x - y` then `y = y + 1` - the second line doesn't affect the first |
| **Where the `return` is** | A `return` inside a loop ends the **whole function** right away | Linear search returns on the first match |
| **Swap needs a temp** | `a = b; b = a` loses the old `a` | Use `temp = a; a = b; b = temp` |
| **Print before vs after a recursive call** | Changes the output order completely | See worked example 3 |

---

## 3. How to Trace: The Trace Table

**Never trace in your head.** Make a table with one column per variable and one row per step. Write down every change. It's slow at first and becomes fast with practice.

### Worked example 1: a loop

```
FindMax(numbers, numbersSize) {
   max = numbers[0]
   for (i = 1; i < numbersSize; ++i) {
      if (numbers[i] > max) {
         max = numbers[i]
      }
   }
   return max
}
```
Call: `FindMax([3, 7, 2, 9, 4], 5)`

| Step | i | numbers[i] | numbers[i] > max? | max |
|---|---|---|---|---|
| start | - | - | - | 3 |
| 1 | 1 | 7 | 7 > 3 yes | 7 |
| 2 | 2 | 2 | 2 > 7 no | 7 |
| 3 | 3 | 9 | 9 > 7 yes | 9 |
| 4 | 4 | 4 | 4 > 9 no | 9 |
| end | 5 | loop stops (5 < 5 is false) | | **return 9** |

### Worked example 2: a while loop with two variables

```
x = 10
count = 0
while (x > 1) {
   x = x / 2      // integer division
   count = count + 1
}
print count
```

| Step | x (before) | x > 1? | x (after) | count |
|---|---|---|---|---|
| 1 | 10 | yes | 5 | 1 |
| 2 | 5 | yes | 2 | 2 |
| 3 | 2 | yes | 1 | 3 |
| 4 | 1 | **no, stop** | | |

Output: **3**. (The loop halves `x` each time, so it's O(log n). This "halving" pattern is the fingerprint of binary search.)

### Worked example 3: recursion (use a call stack)

For recursion, write each call on its own line, indented, and fill in the return values on the way back **up**.

```
PrintUp(n) {
   if (n == 0) {
      return
   }
   PrintUp(n - 1)
   print n
}
```
Call: `PrintUp(3)`

```
PrintUp(3)
   PrintUp(2)
      PrintUp(1)
         PrintUp(0)  -> base case, return
         print 1
      print 2
   print 3
```
Output: **1 2 3**. The `print` happens **after** the recursive call returns, so the deepest call prints first.

If the `print n` line were **above** `PrintUp(n - 1)`, the output would be **3 2 1**. Same lines, different order, opposite output.

Returning values works the same way:

```
Sum(n) {
   if (n == 1) {
      return 1
   }
   return n + Sum(n - 1)
}
```
```
Sum(4) = 4 + Sum(3)
             Sum(3) = 3 + Sum(2)
                          Sum(2) = 2 + Sum(1)
                                       Sum(1) = 1      <- base case
                          Sum(2) = 2 + 1 = 3
             Sum(3) = 3 + 3 = 6
Sum(4) = 4 + 6 = 10
```

---

## 4. Reading Big O off Pseudocode

You don't need math for most questions. Look at **how the loop variable changes** and **how loops are arranged**.

| Pattern in the pseudocode | Big O | Why |
|---|---|---|
| No loops, no recursion | O(1) | Fixed number of steps |
| `for (i = 0; i < n; ++i)` | O(n) | Runs n times |
| `i = i * 2` or `i = i / 2` inside the loop | O(log n) | Doubling/halving reaches n in log₂ n steps |
| A loop inside a loop, both up to n | O(n²) | n × n |
| Inner loop starts at `j = i` (or `i + 1`) | still O(n²) | Runs n + (n-1) + ... + 1 = n(n+1)/2 times, and constants drop |
| Two loops **one after the other** (not nested) | O(n) | n + n = 2n → O(n) |
| Loop to n, with a halving loop inside | O(n log n) | n × log n |
| Recursion that splits in half and does O(n) work to combine | O(n log n) | Merge sort |
| Recursion that calls itself **twice** on n - 1 | O(2ⁿ) | Naive Fibonacci |
| Recursion that calls itself once on n - 1 | O(n) | Factorial, Sum above |

**Rule of thumb:** nested = multiply, sequential = add, then keep only the biggest term.

---

## 5. The 4-Step Routine for Any Pseudocode Question

1. **Read the signature.** What goes in, what comes out? (`BinarySearch(numbers, numbersSize, key)` → returns an index)
2. **Find the loops and the `return`s.** These are the skeleton. What controls each loop? When does the function exit early?
3. **Trace a tiny input** (3-5 elements) with a trace table.
4. **Name it and size it.** Does it match an algorithm fingerprint ([02_algorithm_fingerprints](02_algorithm_fingerprints.md))? What's the loop pattern (section 4)?

---

## Self-check questions
1. How many times does `for (i = 2; i <= 6; ++i)` run?
2. What is `(3 + 8) / 2` in integer division?
3. A `return` is inside a `for` loop. What happens when it runs?
4. What is the Big O of a loop where `i` starts at `n` and does `i = i / 2` until `i < 1`?
5. Two separate (not nested) loops each run n times. What's the Big O?
6. In a recursive function, does a `print` placed **after** the recursive call print in increasing or decreasing order of depth?

<details>
<summary>Answers</summary>

1. 5 times (i = 2, 3, 4, 5, 6).
2. 5 (11 / 2 = 5.5, decimal dropped).
3. The whole function ends immediately and returns that value. The loop doesn't finish.
4. O(log n).
5. O(n). n + n = 2n, and constants drop.
6. The deepest call prints first, so the output comes out in reverse order of the calls (e.g. `PrintUp(3)` prints 1 2 3).
</details>

### My notes
-
