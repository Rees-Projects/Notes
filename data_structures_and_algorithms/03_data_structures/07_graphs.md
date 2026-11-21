# Graphs

> **Layer:** Graph is an ADT (vertices + edges) | **Data structures:** adjacency list (array/dict of lists), adjacency matrix (2D array), edge list | **Traversal uses:** [Queue](../02_abstract_data_types/04_queue_and_deque.md) for BFS, [Stack](../02_abstract_data_types/03_stack.md) for DFS, [Set](../02_abstract_data_types/05_set_and_map.md) for visited

## What is a Graph?
A collection of **vertices (nodes)** and **edges (links)** connecting them. Can be directed/undirected, weighted/unweighted, cyclic/acyclic.

**Notation:** G = (V, E) where V = vertices, E = edges.

## Graph Terminology
- **Vertex/Node** - Entity
- **Edge** - Connection between vertices (u,v)
- **Directed Graph (Digraph)** - Edges have direction (u → v)
- **Undirected Graph** - Edge (u,v) bidirectional
- **Weighted Graph** - Edges have weights/costs (e.g., distance)
- **Unweighted** - All edges equal weight
- **Connected** - Path exists between any two vertices (undirected)
- **Strongly Connected** - Every vertex reachable from every other (directed)
- **Acyclic** - No cycles (DAG = Directed Acyclic Graph)
- **Cycle** - Path returns to start vertex
- **Adjacency** - Two vertices connected by edge
- **Degree** - #edges incident (undirected). In-degree/out-degree (directed)
- **Path/Traversal** - Sequence of vertices via edges

## Graph Representations

### 1. Adjacency List (Most Common)
Array/list of lists: `graph[u]` contains neighbors v.

- **Space:** O(V + E)
- **Add edge:** O(1)
- **Check edge (u,v):** O(degree(u)) or O(k)
- **Iterate neighbors:** O(degree(u))
- **Good for:** Sparse graphs, traversal (BFS/DFS)

### 2. Adjacency Matrix
2D array `A[u][v] = 1` (or weight) if edge exists.

- **Space:** O(V²) - wasteful for sparse
- **Add edge:** O(1)
- **Check edge:** O(1)
- **Iterate neighbors:** O(V) per vertex
- **Good for:** Dense graphs, O(1) edge lookup

### 3. Edge List
List of (u,v,weight) tuples.

- **Space:** O(E)
- **Good for:** MST algorithms (Kruskal’s) - sort edges

## Python - Adjacency List Examples

**Undirected Graph:**
```python
# Adjacency list
graph = {
    'A': ['B', 'C'],
    'B': ['A', 'D'],
    'C': ['A', 'E'],
    'D': ['B', 'E', 'F'],
    'E': ['C', 'D', 'F'],
    'F': ['D', 'E']
}
```

**Directed Graph:**
```python
graph_dir = {
    'A': ['B'],
    'B': ['C', 'D'],
    'C': ['D'],
    'D': []
}
```

**Weighted Graph:**
```python
# (neighbor, weight)
graph_w = {
    'A': [('B',2), ('C',4)],
    'B': [('A',2), ('D',3)],
    'C': [('A',4), ('D',1)],
    'D': [('B',3), ('C',1)]
}
```

## Graph Traversals

### BFS (Breadth-First Search)
Explore level by level (shortest path in **unweighted** graphs). Uses **Queue (FIFO)** + visited set.

**Use cases:** Shortest path in unweighted, shortest in edges, peer-level exploration.

```python
from collections import deque

def bfs(graph, start):
    visited = set([start])
    q = deque([start])
    order = []
    while q:
        cur = q.popleft()
        order.append(cur)
        # visit neighbors (order doesn't matter)
        for nbr in graph.get(cur, []):
            if nbr not in visited:
                visited.add(nbr)
                q.append(nbr)
    return order
```

**BFS to find shortest path (unweighted):**
```python
def bfs_shortest_path(graph, start, target):
    if start == target: return [start]
    visited = {start}
    q = deque([(start, [start])])  # (node, path)
    while q:
        cur, path = q.popleft()
        for nbr in graph.get(cur, []):
            if nbr not in visited:
                if nbr == target:
                    return path + [nbr]
                visited.add(nbr)
                q.append((nbr, path + [nbr]))
    return None
```

### DFS (Depth-First Search)
Go deep first, backtrack when stuck. Uses **Stack (LIFO)** or **recursion** + visited.

**Use cases:** Path finding, cycle detection, topological sort, connected components, backtracking.

**Recursive DFS:**
```python
def dfs(graph, start):
    visited = set()
    order = []
    
    def _dfs(node):
        visited.add(node)
        order.append(node)
        for nbr in graph.get(node, []):
            if nbr not in visited:
                _dfs(nbr)
    _dfs(start)
    return order
```

**Iterative DFS (stack):**
```python
def dfs_iter(graph, start):
    visited = set([start])
    stack = [start]  # push/pop end
    order = []
    while stack:
        cur = stack.pop()
        order.append(cur)
        # push neighbors in reverse order if want same as recursive
        for nbr in reversed(graph.get(cur, [])):
            if nbr not in visited:
                visited.add(nbr)
                stack.append(nbr)
    return order
```

**Note:** Iterative DFS order can differ slightly by neighbor push order vs recursive; both valid DFS.

## Connected Components

**Undirected graph:** Find separate islands/components.

```python
def count_components(graph):
    visited = set()
    count = 0
    for node in graph:
        if node not in visited:
            # traverse component
            count += 1
            # BFS/DFS from node
            stack = [node]
            visited.add(node)
            while stack:
                cur = stack.pop()
                for nbr in graph[cur]:
                    if nbr not in visited:
                        visited.add(nbr)
                        stack.append(nbr)
    return count
```

## Cycle Detection

### Undirected Graph
Cycle exists if we reach a visited neighbor that's **not parent**.

**DFS approach:**
```python
def has_cycle_undirected(graph):
    visited = set()
    def dfs(node, parent):
        visited.add(node)
        for nbr in graph.get(node, []):
            if nbr in visited and nbr != parent:
                return True
            if nbr not in visited and dfs(nbr, node):
                return True
        return False
    for n in graph:
        if n not in visited:
            if dfs(n, None): return True
    return False
```

### Directed Graph
Cycle detection via DFS with **3 states** (unvisited, visiting/in-stack, visited) or **Kahn’s algorithm** for topological (count indegrees).

A cycle exists if DFS reaches a node that is still **VISITING** (on the current path) - that is a back edge. (The undirected "not parent" trick doesn't work here: in a directed graph, reaching an already-**DONE** node is not a cycle.)

```python
def has_cycle_directed(graph):
    UNVISITED, VISITING, DONE = 0, 1, 2
    state = {node: UNVISITED for node in graph}

    def dfs(node):
        state[node] = VISITING
        for nbr in graph.get(node, []):
            if state.get(nbr, UNVISITED) == VISITING:
                return True                      # back edge -> cycle
            if state.get(nbr, UNVISITED) == UNVISITED and dfs(nbr):
                return True
        state[node] = DONE
        return False

    return any(state[n] == UNVISITED and dfs(n) for n in graph)

assert has_cycle_directed({'A': ['B'], 'B': ['C'], 'C': ['A']})
assert not has_cycle_directed({'A': ['B', 'C'], 'B': ['C'], 'C': []})
```

## BFS vs DFS

| Aspect | BFS | DFS |
|---|---|---|
| Data Structure | Queue (FIFO) | Stack (LIFO)/Recursion |
| Order | Level by level | Depth first |
| Shortest Path (unweighted) | Guaranteed | Not guaranteed |
| Memory | O(V) - width | O(h) - height (tree) or O(V) worst |
| Use | Shortest in edges, levels | Backtracking, cycle detection, topo |

## Real-World Graph Examples
- Social networks (users, friendships)
- Maps/roads (cities, distances) - weighted
- Web (pages, links) - directed
- Dependencies (build systems) - DAG
- GPS, routing (Dijkstra, A*)
- Recommendation, networks
