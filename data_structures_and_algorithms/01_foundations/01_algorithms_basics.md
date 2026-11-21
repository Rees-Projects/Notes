# Algorithms Basics - C949

## What is an Algorithm?
An algorithm is a **well-defined, step-by-step procedure** or set of instructions to solve a specific problem, process data, or perform a computation.

- A finite sequence of unambiguous instructions
- Takes input (possibly none), produces output
- Terminates after a finite number of steps

## Characteristics of a Good Algorithm
1. **Correctness** - Solves the problem correctly for all valid inputs
2. **Clarity/Unambiguity** - Each step is clear and unambiguous
3. **Finiteness** - Must terminate in finite time
4. **Efficiency** - Uses minimal time and space resources (measured by time and space complexity)
5. **Effectiveness** - Each step can be performed in practice
6. **Generality** - Should work for a range of inputs

## Algorithm vs Program
- **Algorithm** = Logical/abstract solution (pseudocode, flowcharts, natural language)
- **Program** = Concrete implementation in a programming language (Python, Java)

## Abstract Data Types (ADTs)

An **Abstract Data Type (ADT)** defines **what operations can be performed** on a data type, but **not how those operations are implemented**. It separates the logical view (interface) from the concrete implementation.

- **ADT = Specification** (behavior, operations, rules)
- **Data Structure = Implementation** (how we store and implement those operations in code)

**Example:** A "List" ADT specifies operations like `get(i)`, `insert(i, value)`, `remove(i)`, `size()`. Arrays and Linked Lists are different *data structures* that can both implement the List ADT.

The full table of common ADTs and the data structures that implement them is in [ADT Overview](../02_abstract_data_types/01_adt_overview.md).

## How to Describe Algorithms
- Natural language
- Pseudocode
- Flowcharts
- Programming code

## Example: Find Maximum in List

**Pseudocode:**
```
Input: list A of n elements
max = A[0]
for i from 1 to n-1:
  if A[i] > max: max = A[i]
return max
```

**Python:**
```python
def find_max(arr):
    if not arr:
        raise ValueError("Empty array")
    max_val = arr[0]
    for num in arr[1:]:
        if num > max_val:
            max_val = num
    return max_val
```

**Java (for comparison):**
```java
public static int findMax(int[] arr) {
    if (arr.length == 0) throw new IllegalArgumentException("Empty array");
    int maxVal = arr[0];
    for (int i = 1; i < arr.length; i++) {
        if (arr[i] > maxVal) maxVal = arr[i];
    }
    return maxVal;
}
```

## Complexity of an Algorithm

### Time Complexity
The **amount of time required** for an algorithm to finish executing and produce an output. Also described as how the execution time grows relative to the input size.

- Measures **runtime efficiency**
- Expressed using **asymptotic notation** (Big O, Omega, Theta)
- Focuses on growth rate, not exact milliseconds (machine-independent)

### Space Complexity
The **quantity of memory/space required** for an algorithm to solve a problem and produce an output.

- Includes **input space** + **auxiliary (extra) space** used by the algorithm
- Measures **memory efficiency**
- Also expressed in asymptotic notation (e.g., O(1), O(n), O(n²))

**Trade-off:** Often improving time increases space and vice versa (time-space tradeoff).

## Examples of Time & Space Complexity

### Example 1: Sum of Array Elements

**Algorithm (Iterative):**
```python
def sum_array(arr):
    total = 0        # O(1) space
    for num in arr:  # loop runs n times -> O(n) time
        total += num
    return total
```

**Time Complexity:** O(n) - Time increases linearly with input size n (must visit each element once).

**Space Complexity:** O(1) - Only uses `total` and loop variable (constant extra space, excludes input array).

### Example 2: Creating a Copy with Duplicates

**Algorithm:**
```python
def duplicate_array(arr):
    result = []           # O(n) space for result
    for num in arr:       # O(n) time
        result.append(num * 2)
    return result
```

**Time Complexity:** O(n) - Must process each element once.

**Space Complexity:** O(n) - Creates a new array `result` of size n to produce the output (proportional to input).

### Example 3: Nested Loops (Quadratic Time)

**Algorithm:**
```python
def print_pairs(arr):
    for i in range(len(arr)):        # O(n)
        for j in range(len(arr)):    # O(n) -> nested = O(n²)
            print(arr[i], arr[j])
```

**Time Complexity:** O(n²) - Execution time grows quadratically as n increases.

**Space Complexity:** O(1) - Only loop variables, no extra data structure of size n.

**Java Comparison (Same Complexity):**
```java
public static void printPairs(int[] arr) {
    for (int i = 0; i < arr.length; i++) {      // O(n)
        for (int j = 0; j < arr.length; j++) {  // O(n)
            System.out.println(arr[i] + "," + arr[j]);
        }
    }
}
// Time: O(n²), Space: O(1)
```

## Key Takeaways
- Algorithms focus on "how to solve" - steps and logic
- Different implementations (Python vs Java) can use same algorithm
- Correctness and efficiency are separate concerns
- Efficiency is evaluated by **time complexity** (time to execute/finish) and **space complexity** (memory required to solve problem and produce output)
- Time complexity = how long it takes (growth with input). Space complexity = how much memory it uses (growth with input).
