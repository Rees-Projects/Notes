# Problem Solving & Algorithm Analysis

## Problem Solving Process (I.Q.O.C.E. - Input, Question, Output, Constraints, Examples)

A solid way to remember: **Input → Question/Problem → Output → Constraints → Examples**. Clarify these first before coding.

1. **I - Input** - What data is given? (type, structure, format, edge cases)
2. **Q - Question/Problem** - What exactly needs to be solved/computed? (requirements, assumptions)
3. **O - Output** - What should be returned? (type, format, expected result)
4. **C - Constraints** - What are the limits? (size n, value ranges, time/memory limits, empty inputs)
5. **E - Examples** - Write/sample I/O to confirm understanding (small test cases, edge cases)
6. **Define/plan solution** - Identify approach, data structures, algorithm
7. **Design algorithm** - Write steps (pseudocode), consider edge cases
8. **Verify correctness** - Test against examples (I→O mapping)
9. **Analyze efficiency** - Time and space complexity
10. **Implement** - Code in chosen language
11. **Test & debug** - Edge cases, sample I/O, performance
12. **Optimize if needed** - Tradeoffs

**Example using I.Q.O.C.E.:**
- **Input**: Array `[1,2,3,4,5]`
- **Question**: Find sum of all elements
- **Output**: `15`
- **Constraints**: n >= 0, integers (possibly negative/positive)
- **Examples**: `[]` → `0` (if empty), `[5]` → `5`, `[-1,1]` → `0`

## Thinking Like an Engineer: Problem → Tradeoffs

After clarifying the problem (I.Q.O.C.E. above), reason through the solution in this order:

1. **Problem** - What must be solved?
2. **Solution strategy (algorithm)** - What approach solves it?
3. **Required behavior (ADT)** - What behavior does the strategy need from its data? (the *what*)
4. **Implementation (data structure)** - What concrete structure provides that behavior? (the *how*)
5. **Operations** - What operations will the algorithm call on it?
6. **Runtime (Big O)** - What does each operation cost? What's the total?
7. **Why - tradeoffs** - Why this choice over the alternatives? (time vs space, average vs worst case, simplicity, ordering)

**Worked example - "Is this string of brackets balanced?"**
1. Problem: given `"({[]})"`, return True if every opener is closed in the correct order
2. Strategy: scan left to right. Remember openers, and match each closer to the **most recent** unmatched opener
3. ADT: "most recent first" = **Stack (LIFO)**
4. Data structure: Python `list` (dynamic array)
5. Operations: `append` (push) on an opener, `pop` on a closer, check empty at the end
6. Runtime: push/pop O(1) → O(n) time for the whole string, O(n) space in the worst case (all openers)
7. Tradeoffs: a linked-list stack has the same Big O but more memory overhead per item. A `list` is simpler and cache-friendly. A counter variable would use O(1) space, but it only works for **one** bracket type, so the stack is needed for `()[]{}`

Full code: [Stacks - Balanced Parentheses](../02_abstract_data_types/03_stack.md#example-balanced-parentheses). More applied examples: [README - The Framework Applied](../README.md#the-framework-applied).

## What is Algorithm Analysis?
Measure performance of algorithms independent of hardware, language, implementation details. Focus on **growth rate** as input size (n) increases.

Two main resources:
- **Time complexity** - How execution time grows with input
- **Space complexity** - How memory usage grows with input

## Best Case, Average Case, Worst Case
- **Best case** - Fastest scenario (e.g., target first element)
- **Average case** - Expected performance over random inputs
- **Worst case** - Slowest (upper bound) - most important for guarantees

## Asymptotic Notation

**Important:** Big O / Omega / Theta are **bounds on a function's growth**. Best / average / worst case are **which input scenario** you are measuring. They are separate ideas - you can state a Big O, Omega, or Theta for *any* case. (e.g., binary search's worst case is Θ(log n); its best case is Θ(1).) In practice people usually quote Big O of the worst case, which is why the two get mixed up.

### Big O - O(f(n)) - Upper Bound
Running time grows **no faster than** f(n) (for large n, ignoring constants). "At most this much."

Examples:
- O(1) - Constant (array access, assignment)
- O(log n) - Logarithmic (binary search)
- O(n) - Linear (single loop)
- O(n log n) - Linearithmic (merge sort, heap sort)
- O(n²) - Quadratic (nested loops, bubble/selection)
- O(2ⁿ) - Exponential (recursive subsets)
- O(n!) - Factorial (permutations)

### Omega - Ω(f(n)) - Lower Bound
Running time grows **at least as fast as** f(n). "At least this much."

### Theta - Θ(f(n)) - Tight Bound
Both O(f(n)) **and** Ω(f(n)) hold - growth is exactly f(n) (up to constants). e.g., merge sort is Θ(n log n) in every case.

## Common Growth Rates (Slowest-Growing → Fastest-Growing)
Listed from most efficient (grows slowest) to least efficient (grows fastest):
O(1) < O(log n) < O(n) < O(n log n) < O(n²) < O(n³) < O(2ⁿ) < O(n!)

## Space Complexity
Includes:
- **Auxiliary space** - Extra space algorithm uses (excluding input)
- **Total space** - Input + auxiliary

Usually analyze auxiliary space or total. Consider recursion stack (O(n) for deep recursion).

## Python Examples

**O(1) - Constant:**
```python
def get_first(arr):
    return arr[0] if arr else None  # single access
```

**O(n) - Linear:**
```python
def sum_array(arr):
    total = 0
    for num in arr:  # loops once
        total += num
    return total
```

**O(n²) - Quadratic:**
```python
def has_duplicates(arr):
    n = len(arr)
    for i in range(n):
        for j in range(i+1, n):
            if arr[i] == arr[j]:
                return True
    return False
```

**O(log n) - Logarithmic:** Binary search - halves the search range each step. Full code in [Searching](../04_algorithms/02_searching.md#binary-search).

## Java Comparison (for reference)
Same Big O - asymptotic analysis language-independent. Implementation details vary, but growth rates identical.
