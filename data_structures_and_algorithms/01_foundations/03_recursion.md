# Recursion

## What is Recursion?
A function that **calls itself** to solve a smaller subproblem of the original problem. Must have a **base case** to stop.

**Key components:**
1. **Base case(s)** - Termination condition(s) (prevents infinite recursion)
2. **Recursive case(s)** - Call function on smaller input (toward base case)

## Recursion vs Iteration
| Aspect | Recursion | Iteration (Loops) |
|---|---|---|
| Code | Often shorter, cleaner | Often longer |
| Readability | Good for tree/DFS problems | Good for linear |
| Memory | Uses call stack (extra space) | Less stack space |
| Performance | Overhead of calls | Usually faster |
| Risk | Stack overflow (deep recursion) | Infinite loop risk, no stack overflow |

## How Recursion Works
Each recursive call pushed onto **call stack** (LIFO). When base case reached, returns back up (unwinding).

Python recursion limit: `sys.setrecursionlimit` for deep recursion (trees can be deep).

## Examples

### Factorial
n! = n * (n-1)! with base case 0! = 1, 1! = 1

**Recursive:**
```python
def factorial(n):
    if n < 0: raise ValueError
    if n <= 1: return 1  # base case
    return n * factorial(n-1)  # recursive case O(n) time, O(n) space (stack)
```

**Iterative:**
```python
def factorial_iter(n):
    res = 1
    for i in range(2, n+1): res *= i
    return res  # O(n) time, O(1) space
```

### Fibonacci
F(0)=0, F(1)=1, F(n)=F(n-1)+F(n-2)

**Naive Recursive (Inefficient):**
```python
def fib(n):
    if n <= 1: return n  # base
    return fib(n-1) + fib(n-2)  # O(2^n) - exponential, lots of overlap
```

**Memoization (Top-Down DP):**
```python
def fib_memo(n, memo=None):
    if memo is None: memo = {}
    if n in memo: return memo[n]
    if n <= 1: return n
    memo[n] = fib_memo(n-1, memo) + fib_memo(n-2, memo)
    return memo[n]  # O(n) time, O(n) space
```

**Iterative (Best):**
```python
def fib_iter(n):
    if n <= 1: return n
    a,b = 0,1
    for _ in range(2, n+1):
        a,b = b, a+b
    return b  # O(n) time, O(1) space
```

### Sum of Array
```python
def sum_arr(arr):
    if not arr: return 0  # base
    return arr[0] + sum_arr(arr[1:])
```
**Watch out:** `arr[1:]` copies the rest of the list on every call (O(n) each), so this is **O(n²) time and O(n²) space** overall - not O(n). Pass an index instead to get O(n) time (O(n) stack space):

```python
def sum_arr_idx(arr, i=0):
    if i == len(arr): return 0  # base
    return arr[i] + sum_arr_idx(arr, i + 1)  # O(n) time, O(n) stack
```

### Reverse String
```python
def reverse_str(s):
    if len(s) <= 1: return s
    return reverse_str(s[1:]) + s[0]  # O(n²) - slicing + concatenation copy each call
```

### Count Down
```python
def countdown(n):
    if n <= 0:
        print("Done")
        return
    print(n)
    countdown(n-1)
```

## Recursion Tree
Visualizes calls. Helpful for naive Fibonacci (shows overlapping subproblems).

## Tail Recursion
Recursive call is the **last operation** in function (no work after). Some languages optimize (tail-call optimization), Python does **not** optimize tail recursion by default (still uses stack).

```python
# Not tail recursive (has + after)
def fact(n): return 1 if n<=1 else n*fact(n-1)

# Tail recursive form (can be converted to loop)
def fact_tail(n, acc=1):
    if n <= 1: return acc
    return fact_tail(n-1, n*acc)  # last call
```

## When to Use Recursion
- Tree traversals (inorder/preorder/postorder)
- Graph DFS
- Divide & Conquer (merge sort, quicksort)
- Backtracking (permutations, combinations)
- Problems naturally recursive (factorial, Fibonacci definition)

## Recursion Pitfalls
- Missing/incorrect base case → infinite recursion → RecursionError (StackOverflow)
- Not reducing problem size → infinite
- Recomputing subproblems (overlapping) → exponential → use memoization/DP

## Java Comparison
```java
int factorial(int n){
    if(n<=1) return 1;
    return n*factorial(n-1);
}
// Deep recursion can hit StackOverflowError (can increase stack or convert to iterative)
```

## Key Takeaways
- Always define base case first
- Ensure progress toward base case
- Watch for overlapping subproblems (consider DP)
- For very deep n (trees), consider iterative or recursionlimit/memo
