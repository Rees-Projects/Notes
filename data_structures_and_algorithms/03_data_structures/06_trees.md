# Trees

> **Layer:** Tree is an ADT (hierarchy) **and** a family of data structures | **Built from:** nodes + pointers (or an array for complete trees, like heaps) | **BST implements:** sorted [Map / Set](../02_abstract_data_types/05_set_and_map.md) | **Binary heap** (a tree in an array): see [Binary Heaps](05_binary_heaps.md)

## What is a Tree?
A **non-linear**, hierarchical data structure with nodes connected by edges. Has a **root**, parent-child relationships, and no cycles (acyclic connected graph for general trees).

**Tree Terminology:**
- **Root** - Top node (only one)
- **Node** - Element (data + children references)
- **Edge** - Link between nodes
- **Parent/Child** - Relationship
- **Leaf** - Node with no children
- **Internal node** - Has >=1 child
- **Subtree** - Tree rooted at some node
- **Path** - Sequence from node to ancestor/descendant
- **Depth** - Distance from root (root depth 0 or 1; be consistent)
- **Height** - Max depth of any leaf (or edges on longest downward path)
- **Level** - Root at level 0 (or 1)
- **Size** - Total number of nodes

## Binary Tree
Each node has at most **2 children**: left and right.

**Types of Binary Trees:**
- **Full (Proper)** - Every node has 0 or 2 children (no node with exactly 1)
- **Complete** - All levels filled except possibly last, filled left-to-right
- **Perfect** - All internal nodes have 2, all leaves at same level
- **Balanced** - Height difference small (AVL, Red-Black) → O(log n) ops
- **Skewed** - All nodes go left/right → like linked list, height = n → O(n)

## Binary Search Tree (BST)
Binary tree with ordering property:
- Left subtree values < root value
- Right subtree values > root value
- Both subtrees are BSTs (no duplicates usually)

**Time Complexity (avg):** Search/Insert/Delete O(log n). **Worst (skewed):** O(n)

## Tree Traversals (DFS & BFS)

### DFS (Depth-First Search) - Recursive common
Visit children before siblings (go deep).

**3 Main (Binary):**
1. **Preorder (Root, Left, Right)** - Useful to copy tree, serialize
2. **Inorder (Left, Root, Right)** - For BST gives **sorted ascending** order
3. **Postorder (Left, Right, Root)** - Useful to delete tree (children first)

### BFS (Breadth-First Search) - Level Order
Visit level by level (siblings before going deeper). Uses **Queue**.

- **Level Order** - Left to right per level

## Python Implementation - Binary Tree Node

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right
```

## Traversals - Python

### DFS Recursive

```python
def preorder(root):
    res = []
    def dfs(node):
        if not node: return
        res.append(node.val)        # root
        dfs(node.left)              # left
        dfs(node.right)             # right
    dfs(root)
    return res

def inorder(root):
    res = []
    def dfs(node):
        if not node: return
        dfs(node.left)
        res.append(node.val)        # root
        dfs(node.right)
    dfs(root)
    return res

def postorder(root):
    res = []
    def dfs(node):
        if not node: return
        dfs(node.left)
        dfs(node.right)
        res.append(node.val)        # root
    dfs(root)
    return res
```

### BFS (Level Order) - Iterative with Queue

```python
from collections import deque

def level_order(root):
    if not root: return []
    res, q = [], deque([root])
    while q:
        node = q.popleft()
        res.append(node.val)
        if node.left: q.append(node.left)
        if node.right: q.append(node.right)
    return res
```

### DFS Iterative (Preorder example)

```python
def preorder_iter(root):
    if not root: return []
    res, stack = [], [root]
    while stack:
        node = stack.pop()  # LIFO
        res.append(node.val)
        # Push right first so left is processed first (popped last? stack: push right, left -> pop left)
        if node.right: stack.append(node.right)
        if node.left: stack.append(node.left)
    return res
```

## BST Operations (Basic)

```python
class BST:
    def __init__(self):
        self.root = None
    
    def insert(self, val):
        def _ins(node, v):
            if not node: return TreeNode(v)
            if v < node.val:
                node.left = _ins(node.left, v)
            else:  # allow duplicates? usually > or handle
                node.right = _ins(node.right, v)
            return node
        self.root = _ins(self.root, val)
    
    def search(self, val):
        cur = self.root
        while cur:
            if val == cur.val: return True
            cur = cur.left if val < cur.val else cur.right
        return False
```

## Height & Count

```python
def height(root):
    if not root: return -1  # or 0 depending on definition
    return 1 + max(height(root.left), height(root.right))
```

## When to Use Trees
- Hierarchical data (DOM, file systems, org charts)
- BST for sorted dynamic data (search/insert/delete)
- Binary Heaps (priority queues) - heap is tree-based
- Trie (prefix trees) for autocomplete
- Expression trees

## Java Comparison
Nodes same concept. Traversals identical logic.
