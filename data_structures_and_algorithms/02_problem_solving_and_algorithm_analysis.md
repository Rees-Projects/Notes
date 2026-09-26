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

### Big O - O(n) - Upper Bound (Worst Case)
Describes worst-case growth. "Algorithm runs at most this fast-growing function".

Examples:
- O(1) - Constant (array access, assignment)
- O(log n) - Logarithmic (binary search)
- O(n) - Linear (single loop)
- O(n log n) - Linearithmic (merge sort, heap sort)
- O(n²) - Quadratic (nested loops, bubble/selection)
- O(2ⁿ) - Exponential (recursive subsets)
- O(n!) - Factorial (permutations)

### Omega - Ω(n) - Lower Bound (Best Case)
Minimum time the algorithm will take.

### Theta - Θ(n) - Tight Bound
When best and worst are the same class (average ≈ worst).

## Common Growth Rates (Slowest to Fastest growth? Or fastest to slowest)
As n grows: O(1) < O(log n) < O(n) < O(n log n) < O(n²) < O(n³) < ... < O(2ⁿ)

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

**O(log n) - Logarithmic:**
```python
def binary_search(arr, target):
    left, right = 0, len(arr)-1
    while left <= right:
        mid = (left + right) // 2
        if arr[mid] == target: return mid
        if arr[mid] < target: left = mid + 1
        else: right = mid - 1
    return -1
```

## Java Comparison (for reference)
Same Big O - asymptotic analysis language-independent. Implementation details vary, but growth rates identical.
