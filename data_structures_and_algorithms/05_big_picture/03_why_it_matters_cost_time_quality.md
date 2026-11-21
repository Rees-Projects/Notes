# Why This Matters for a Software Engineer - Cost, Time, Quality

Knowing the ADT, the data structure, and the Big O is only half the job. A software engineer also has to **justify** a choice to a team, a manager, or a client. That justification is made with the **Cost - Time - Quality triangle**.

---

## The Triangle

```
                         QUALITY
                            /\
                           /  \
                          /    \
                         /      \
                        /        \
                       /  BALANCED \
                      /   PROJECT   \
                     /                \
                    /__________________\
                COST                    TIME
```

All three sides are **equal**: an equilateral triangle. That means the three constraints are **linked and equally important**.

- **Change one side and at least one other has to move** to keep the triangle intact.
- The common shorthand is "**Fast, cheap, good - pick two**":
  - Fast + cheap → quality suffers
  - Fast + good → costs more (more people, better infrastructure)
  - Cheap + good → takes longer

(Some textbooks draw it with **Scope** instead of Quality, or with quality in the middle. The idea is the same: the constraints trade against each other.)

## What Each Side Means in DSA Terms

| Side | Project meaning | How DSA choices affect it |
|---|---|---|
| **Cost** | Money and resources: developer hours, servers, memory, cloud bills | **Space complexity** = RAM you pay for. **Runtime complexity** = CPU/servers you pay for at scale. Custom structures cost developer hours to build and maintain |
| **Time** | Schedule: how fast the feature ships | Built-ins (`dict`, `deque`, `heapq`) ship fast. Writing your own balanced BST or custom hash table takes days, plus testing |
| **Quality** | Correctness, performance, reliability, maintainability | Big O decides whether it **still works at 10× the data**. Guarantees (worst case) give reliability. Simple, standard code is maintainable |

**Don't mix up the two kinds of "time":**
- **Time on the triangle** = **development schedule** (how long to build it).
- **Time complexity (Big O)** = **runtime** (how long the program takes to run). That belongs mostly to **Quality**, and at scale also to **Cost** (more servers).

## How the Triangle Justifies an Implementation

The [7-step framework](../README.md#thinking-like-an-engineer) ends at **Step 7: Why - tradeoffs**. The triangle is how you express that "why" in terms a team or manager cares about.

### Justification template
> "For **[problem]**, the algorithm needs **[ADT]** behavior. I chose **[data structure]** because **[key operations]** are **[Big O]**.
> This favors **[side(s)]** at the expense of **[side]**, which is acceptable because **[constraint: data size, deadline, budget, reliability need]**."

### Worked examples

| Decision | Choice | Cost | Time (schedule) | Quality | Justification |
|---|---|---|---|---|---|
| Username lookup for 10M users | Built-in hash set (`set`) | ↑ memory for 10M keys | ✅ ships today (built in) | ✅ O(1) avg checks | Memory is cheap compared to the user-facing slowness of O(n) list checks. Built-in means no extra dev time |
| Sort 50 config items once at startup | Simple `sorted()` or even insertion sort | ✅ none | ✅ minutes | ✅ fine for n = 50 | n is tiny and fixed, so optimizing would add cost and time with no quality gain |
| Leaderboard that must stay sorted under heavy updates | Balanced BST / sorted container library | ↑ library or dev effort | ↓ takes longer than a dict | ✅ O(log n) updates + sorted order | A hash table can't keep order. Re-sorting on every update is O(n log n) and would fail at scale. The extra time is worth the quality |
| Task queue in a prototype due Friday | Python `list` with `pop(0)` | ✅ | ✅ fastest to write | ❌ O(n) dequeue | **Acceptable only as known tech debt** while n is small. Swap to `deque` (a one-line change) before launch. Document it |
| Task queue in production | `collections.deque` | ✅ same memory | ✅ one-line change | ✅ O(1) both ends | No reason to accept lower quality when the better choice costs nothing extra |
| Hospital triage system | Binary heap priority queue + thorough tests | ↑ testing effort | ↓ more review time | ✅✅ correct priority, O(log n) | Quality is non-negotiable (safety-critical), so time and cost flex |
| Speeding up a slow report with caching (memoization) | Hash map cache | ↑ memory (cost) | ~ small dev effort | ✅ repeated queries go O(n) → O(1) | Classic **time-space tradeoff**: spend memory (cost) to buy runtime performance (quality) |

### Reading the examples
- **Small n** → keep it simple. Optimizing spends Cost and Time without improving Quality much.
- **Large n or growing data** → Big O dominates. A poor choice becomes a Quality failure, and eventually a Cost failure (more servers).
- **Safety- or money-critical** → Quality is fixed, so Cost and Time have to flex.
- **Tight deadline** → use built-ins. A faster-to-write but weaker choice is acceptable only if it is **documented tech debt** with a plan to fix it.

## Connecting to the SDLC
| SDLC phase | Triangle question |
|---|---|
| Plan | What are the budget (cost), deadline (time), and quality bar? Which side is fixed? |
| Analyze | How big is n, and how fast will it grow? (decides how much Big O matters) |
| Design | Pick the ADT + data structure, and write down the triangle justification |
| Implement | Prefer built-ins to protect Time and Cost |
| Test | Verify the Quality claim: edge cases and performance at a realistic n |
| Maintain | Data grew? Revisit the choice, since the balance of the triangle may have shifted |

## Practice: Justify It
Write a one-sentence triangle justification for each:
1. Using an adjacency **list** instead of an adjacency **matrix** for a road map of 50,000 cities.
2. Using **merge sort** instead of **quick sort** for a banking report that must keep equal transactions in their original order.
3. Shipping a demo with a nested-loop O(n²) duplicate check on 100 items.

<details>
<summary>Sample answers</summary>

1. "The graph is sparse (about 4 roads per city), so an adjacency list uses O(V+E) memory instead of 2.5 billion matrix cells. That lowers **cost** a lot and keeps BFS/DFS **quality** at O(V+E), with no extra **time** to build."
2. "Merge sort is **stable** and guaranteed O(n log n), which protects **quality** (correct ordering, no O(n²) worst case). It costs O(n) extra memory, which is an acceptable **cost** for financial correctness."
3. "With n = 100, O(n²) is 10,000 checks, which is instant. Writing it the simple way protects **time** and **cost** for the demo, and **quality** is fine at this size. I'll note it as tech debt to switch to a set (O(n)) if n grows."
</details>
