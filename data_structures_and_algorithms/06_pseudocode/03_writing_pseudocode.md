# Writing Pseudocode

Most algorithms are built from a handful of small **patterns**. If you know the patterns, writing pseudocode means picking the right ones and putting them together, instead of starting from a blank page.

---

## 1. The Recipe

1. **I.Q.O.C.E. first** ([Problem Solving](../01_foundations/02_problem_solving_and_algorithm_analysis.md)). Write the input, output, and 2-3 examples, including an empty or one-element case.
2. **Solve one example by hand** and say out loud what you did. "I looked at each number and kept the biggest one." That sentence **is** your algorithm.
3. **Turn the sentence into a pattern** from the table below.
4. **Write the skeleton:** function name, parameters, loop, return.
5. **Fill in the body.**
6. **Trace your own pseudocode** with your example ([Reading Pseudocode](01_reading_pseudocode.md)). If the trace doesn't match the expected output, fix it.
7. **State the Big O.**

---

## 2. The Building-Block Patterns

| Pattern | Sounds like... | Skeleton |
|---|---|---|
| **Accumulator** | "add up", "total", "product" | `total = 0` → loop → `total = total + x` |
| **Counter** | "how many" | `count = 0` → loop → `if (cond) count = count + 1` |
| **Tracker (max/min)** | "largest", "smallest", "best" | `best = first item` → loop → `if (x > best) best = x` |
| **Search & exit** | "find", "does it contain", "first" | loop → `if (match) return i` → after loop `return -1` |
| **Check all** | "are all...", "is it sorted" | loop → `if (rule broken) return false` → after loop `return true` |
| **Swap** | "exchange", "reverse" | `temp = a`, `a = b`, `b = temp` |
| **Two pointers** | "from both ends", "reverse", "palindrome" | `left = 0`, `right = n - 1`, `while (left < right)` move both inward |
| **All pairs** | "any two", "duplicates", "compare each with each" | nested loops, `j` starts at `i + 1` |
| **Halving** | "sorted", "fast lookup", "guess the number" | `low`, `high`, `mid` |
| **Recursion** | "same problem, smaller" | `if (base case) return answer` → `return combine(n, F(smaller))` |

### Examples of each

**Accumulator - sum of an array**
```
Sum(numbers, numbersSize) {
   total = 0
   for (i = 0; i < numbersSize; ++i) {
      total = total + numbers[i]
   }
   return total
}
```

**Counter - count values greater than a limit**
```
CountAbove(numbers, numbersSize, limit) {
   count = 0
   for (i = 0; i < numbersSize; ++i) {
      if (numbers[i] > limit) {
         count = count + 1
      }
   }
   return count
}
```

**Check all - is the array sorted?**
```
IsSorted(numbers, numbersSize) {
   for (i = 0; i < numbersSize - 1; ++i) {   // stop one early so i + 1 is valid
      if (numbers[i] > numbers[i + 1]) {
         return false
      }
   }
   return true
}
```

**Two pointers - reverse in place**
```
Reverse(numbers, numbersSize) {
   left = 0
   right = numbersSize - 1
   while (left < right) {
      temp = numbers[left]
      numbers[left] = numbers[right]
      numbers[right] = temp
      left = left + 1
      right = right - 1
   }
}
```

**All pairs - does the array have a duplicate?**
```
HasDuplicate(numbers, numbersSize) {
   for (i = 0; i < numbersSize; ++i) {
      for (j = i + 1; j < numbersSize; ++j) {
         if (numbers[i] == numbers[j]) {
            return true
         }
      }
   }
   return false
}
// O(n²). Using a hash set instead makes it O(n) time, O(n) space (see Tradeoffs guide)
```

**Recursion - factorial**
```
Factorial(n) {
   if (n <= 1) {         // base case
      return 1
   }
   return n * Factorial(n - 1)
}
```

---

## 3. Common Mistakes When Writing

| Mistake | Fix |
|---|---|
| Tracker starts at `0` | Start at the **first element**. With `max = 0`, `[-5, -2, -9]` returns 0, which isn't in the array |
| `return false` inside the loop's `else` for a search | Only return "not found" **after** the loop, once everything has been checked |
| Loop goes to `i <= n` | Valid indexes are `0` to `n - 1`. `i <= n` reads past the end |
| Comparing `numbers[i + 1]` with `i < n` | Stop at `i < n - 1`, or `i + 1` goes out of bounds |
| Recursion with no base case (or one never reached) | Every recursive call must move **toward** the base case |
| Forgetting the empty input | Ask "what if `numbersSize` is 0?" before you're done |

---

## 4. Practice: Write These

Write the pseudocode yourself first, then trace it, then check the answer.

1. Return the **average** of an array (assume it's not empty).
2. Return the **index of the smallest** value.
3. Return **how many times** `key` appears.
4. Return `true` if a string is a **palindrome** (`"racecar"`).
5. **Recursively** compute the sum of the digits of a positive integer (`472` → `13`).
6. Return the **second largest** value (assume at least 2 distinct values).

<details>
<summary>Sample answers</summary>

```
1. Average(numbers, numbersSize) {
      total = 0
      for (i = 0; i < numbersSize; ++i) {
         total = total + numbers[i]
      }
      return total / numbersSize
   }
   // Accumulator. O(n)

2. IndexOfMin(numbers, numbersSize) {
      minIndex = 0
      for (i = 1; i < numbersSize; ++i) {
         if (numbers[i] < numbers[minIndex]) {
            minIndex = i
         }
      }
      return minIndex
   }
   // Tracker (of an index, same idea as selection sort's inner loop). O(n)

3. CountKey(numbers, numbersSize, key) {
      count = 0
      for (i = 0; i < numbersSize; ++i) {
         if (numbers[i] == key) {
            count = count + 1
         }
      }
      return count
   }
   // Counter. O(n)

4. IsPalindrome(s, sSize) {
      left = 0
      right = sSize - 1
      while (left < right) {
         if (s[left] != s[right]) {
            return false
         }
         left = left + 1
         right = right - 1
      }
      return true
   }
   // Two pointers + check all. O(n)

5. DigitSum(n) {
      if (n < 10) {
         return n                    // one digit left
      }
      return (n % 10) + DigitSum(n / 10)
   }
   // 472 -> 2 + DigitSum(47) -> 2 + 7 + DigitSum(4) -> 2 + 7 + 4 = 13. O(number of digits)

6. SecondLargest(numbers, numbersSize) {
      largest = numbers[0]
      second = -infinity
      for (i = 1; i < numbersSize; ++i) {
         if (numbers[i] > largest) {
            second = largest          // old largest becomes second
            largest = numbers[i]
         }
         else if (numbers[i] > second && numbers[i] != largest) {
            second = numbers[i]
         }
      }
      return second
   }
   // Two trackers. O(n). Sorting first also works but is O(n log n)
```
</details>

### My notes
-
