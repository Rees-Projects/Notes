# Linked Lists

> **Layer:** Data structure | **Built from:** nodes + pointers | **Implements ADTs:** [List](../02_abstract_data_types/02_list.md), [Stack](../02_abstract_data_types/03_stack.md), [Queue / Deque](../02_abstract_data_types/04_queue_and_deque.md) | **Underlies:** separate chaining in hash tables

## What is a Linked List?
A linear data structure where elements (nodes) are connected via pointers/references. Nodes not stored contiguously in memory (unlike arrays).

**Node structure:**
- Data/value
- Reference to next node (Singly)
- Reference to prev + next (Doubly)

## Types

1. **Singly Linked List (SLL)** - Each node: value + next. Head → ... → Tail (next=None/NULL)
2. **Doubly Linked List (DLL)** - value + prev + next. Allows traversal both directions
3. **Circular Linked List** - Tail.next points back to head

## Arrays vs Linked Lists

| Aspect | Array | Linked List |
|---|---|---|
| Memory | Contiguous | Non-contiguous (nodes scattered) |
| Size | Fixed (static) or resizable cost | Dynamic, grows/shrinks easily |
| Access | O(1) by index | O(n) (traverse from head) |
| Insert/Delete (head) | O(n) | O(1) |
| Insert (tail) | O(1) if space | O(1) with tail pointer |
| Delete (tail) | O(1) | O(n) singly (need prev), O(1) doubly |
| Insert/Delete (middle) | O(n) (search+shift) | O(n) to find + O(1) to link |
| Memory overhead | Less (just values) | More (values + pointers) |
| Cache locality | Good (contiguous) | Poor (scattered) |

## Time Complexity

| Operation | Singly Linked List | Doubly Linked List |
|---|---|---|
| Search | O(n) | O(n) |
| Insert at head | O(1) | O(1) |
| Insert at tail | O(1) with tail ptr, else O(n) | O(1) |
| Insert at position | O(n) | O(n) |
| Delete at head | O(1) | O(1) |
| Delete at tail | O(n) (need prev) or O(1) if tail+prev tracked | O(1) |
| Delete by value/node | O(n) | O(n) |

## Python Implementation - Singly Linked List

```python
class Node:
    def __init__(self, value):
        self.value = value
        self.next = None

class LinkedList:
    def __init__(self):
        self.head = None
        self.tail = None
        self.size = 0
    
    def append(self, value):  # O(1) with tail
        new_node = Node(value)
        if not self.head:
            self.head = self.tail = new_node
        else:
            self.tail.next = new_node
            self.tail = new_node
        self.size += 1
        return self
    
    def prepend(self, value):  # O(1)
        new_node = Node(value)
        if not self.head:
            self.head = self.tail = new_node
        else:
            new_node.next = self.head
            self.head = new_node
        self.size += 1
        return self
    
    def insert_at(self, index, value):  # O(n)
        if index < 0 or index > self.size:
            raise IndexError("Index out of range")
        if index == 0:
            return self.prepend(value)
        if index == self.size:
            return self.append(value)
        new_node = Node(value)
        prev = self._get_node(index - 1)
        new_node.next = prev.next
        prev.next = new_node
        self.size += 1
        return self
    
    def _get_node(self, index):  # O(n)
        curr = self.head
        for _ in range(index):
            curr = curr.next
        return curr
    
    def delete_head(self):  # O(1)
        if not self.head:
            return None
        val = self.head.value
        if self.head == self.tail:
            self.head = self.tail = None
        else:
            self.head = self.head.next
        self.size -= 1
        return val
    
    def delete_tail(self):  # O(n) singly (need prev)
        if not self.head:
            return None
        val = self.tail.value
        if self.head == self.tail:
            self.head = self.tail = None
        else:
            prev = self.head
            while prev.next != self.tail:
                prev = prev.next
            prev.next = None
            self.tail = prev
        self.size -= 1
        return val
    
    def contains(self, value):  # O(n)
        curr = self.head
        while curr:
            if curr.value == value:
                return True
            curr = curr.next
        return False
    
    def __iter__(self):
        curr = self.head
        while curr:
            yield curr.value
            curr = curr.next
    
    def __str__(self):
        return " -> ".join(str(v) for v in self)
```

## Class Version - append / append_node
`append(item)` wraps the raw value in a new node, then hands it to `append_node`. `append_node` checks whether the list is **empty** (`head is None`): if so, the new node becomes both head and tail; otherwise it links after the current tail and becomes the new tail. O(1) because of the tail pointer.

```python
class SinglyLinkedNode:
    def __init__(self, data):
        self.data = data
        self.next = None

class SinglyLinkedList:
    def __init__(self):
        self.head = None
        self.tail = None

    def append(self, item):
        self.append_node(SinglyLinkedNode(item))

    def append_node(self, new_node):
        if self.head is None:        # empty list
            self.head = new_node
            self.tail = new_node
        else:                        # link after current tail
            self.tail.next = new_node
            self.tail = new_node
```

## Java Comparison (Singly Concept)

```java
static class Node {
    int val; Node next;
    Node(int v){ val=v; next=null; }
}

Node head, tail;
void append(int v){
    Node n=new Node(v);
    if(head==null){ head=tail=n; }
    else { tail.next=n; tail=n; }
}
```

## When to Use Linked Lists
- Frequent insertions/deletions at head/tail (no shifting)
- Unknown/dynamic size, grows unpredictably
- Don't need random access by index often
- Avoid if need frequent lookups by index or cache locality critical

## Common Interview Problems
- Reverse linked list (iterative & recursive)
- Detect cycle (Floyd’s tortoise & hare)
- Merge two sorted lists
- Remove nth node from end
- Middle of linked list
