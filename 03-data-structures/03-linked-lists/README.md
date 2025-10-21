# 🔗 Linked Lists - Python DSA

> Master dynamic data structures with pointer manipulation

---

## 📚 Table of Contents

1. [Introduction](#introduction)
2. [Types of Linked Lists](#types-of-linked-lists)
3. [Implementation](#implementation)
4. [Common Patterns](#common-patterns)
5. [Classic Problems](#classic-problems)
6. [Interview Tips](#interview-tips)
7. [Practice Problems](#practice-problems)

---

## Introduction

**Linked List** is a linear data structure where elements are stored in nodes, and each node points to the next node.

### Key Characteristics
- ✅ **Dynamic size** - Grows/shrinks as needed
- ✅ **Efficient insertion/deletion** - O(1) at known position
- ❌ **No random access** - O(n) to access element
- ❌ **Extra memory** - Pointer storage overhead

### Array vs Linked List

| Operation | Array | Linked List |
|-----------|-------|-------------|
| Access by index | O(1) | O(n) |
| Insert at beginning | O(n) | O(1) |
| Insert at end | O(1)* | O(n) or O(1)** |
| Insert in middle | O(n) | O(1)*** |
| Delete | O(n) | O(1)*** |
| Search | O(n) | O(n) |
| Memory | Contiguous | Scattered |

*Amortized  
**With tail pointer  
***With pointer to node

---

## Types of Linked Lists

### 1. Singly Linked List

```
[1] -> [2] -> [3] -> [4] -> None
```

Each node has:
- Data
- Pointer to next node

### 2. Doubly Linked List

```
None <- [1] <-> [2] <-> [3] <-> [4] -> None
```

Each node has:
- Data
- Pointer to next node
- Pointer to previous node

### 3. Circular Linked List

```
[1] -> [2] -> [3] -> [4] -|
 ^________________________|
```

Last node points back to first node.

---

## Implementation

### Node Class

```python
class ListNode:
    """Node for singly linked list."""
    
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next
    
    def __repr__(self):
        return f"ListNode({self.val})"
```

### Singly Linked List

```python
class LinkedList:
    """Singly Linked List implementation."""
    
    def __init__(self):
        self.head = None
        self.size = 0
    
    def insert_at_head(self, val):
        """
        Insert at beginning.
        Time: O(1), Space: O(1)
        """
        new_node = ListNode(val)
        new_node.next = self.head
        self.head = new_node
        self.size += 1
    
    def insert_at_tail(self, val):
        """
        Insert at end.
        Time: O(n), Space: O(1)
        """
        new_node = ListNode(val)
        
        if not self.head:
            self.head = new_node
        else:
            current = self.head
            while current.next:
                current = current.next
            current.next = new_node
        
        self.size += 1
    
    def insert_at_position(self, pos, val):
        """
        Insert at position.
        Time: O(n), Space: O(1)
        """
        if pos == 0:
            self.insert_at_head(val)
            return
        
        new_node = ListNode(val)
        current = self.head
        
        for _ in range(pos - 1):
            if not current:
                raise IndexError("Position out of bounds")
            current = current.next
        
        new_node.next = current.next
        current.next = new_node
        self.size += 1
    
    def delete_at_head(self):
        """
        Delete first node.
        Time: O(1), Space: O(1)
        """
        if not self.head:
            return None
        
        val = self.head.val
        self.head = self.head.next
        self.size -= 1
        return val
    
    def delete_at_tail(self):
        """
        Delete last node.
        Time: O(n), Space: O(1)
        """
        if not self.head:
            return None
        
        if not self.head.next:
            val = self.head.val
            self.head = None
            self.size -= 1
            return val
        
        current = self.head
        while current.next.next:
            current = current.next
        
        val = current.next.val
        current.next = None
        self.size -= 1
        return val
    
    def delete_value(self, val):
        """
        Delete first occurrence of value.
        Time: O(n), Space: O(1)
        """
        if not self.head:
            return False
        
        if self.head.val == val:
            self.head = self.head.next
            self.size -= 1
            return True
        
        current = self.head
        while current.next:
            if current.next.val == val:
                current.next = current.next.next
                self.size -= 1
                return True
            current = current.next
        
        return False
    
    def search(self, val):
        """
        Search for value.
        Time: O(n), Space: O(1)
        """
        current = self.head
        while current:
            if current.val == val:
                return True
            current = current.next
        return False
    
    def get(self, index):
        """
        Get value at index.
        Time: O(n), Space: O(1)
        """
        if index < 0 or index >= self.size:
            raise IndexError("Index out of bounds")
        
        current = self.head
        for _ in range(index):
            current = current.next
        
        return current.val
    
    def to_list(self):
        """Convert to Python list."""
        result = []
        current = self.head
        while current:
            result.append(current.val)
            current = current.next
        return result
    
    def __len__(self):
        return self.size
    
    def __repr__(self):
        return f"LinkedList({self.to_list()})"

# Test
ll = LinkedList()
ll.insert_at_tail(1)
ll.insert_at_tail(2)
ll.insert_at_tail(3)
print(ll)  # LinkedList([1, 2, 3])
```

### Doubly Linked List Node

```python
class DoublyListNode:
    """Node for doubly linked list."""
    
    def __init__(self, val=0, prev=None, next=None):
        self.val = val
        self.prev = prev
        self.next = next
```

---

## Common Patterns

### Pattern 1: Dummy Head

Simplifies edge cases (empty list, single node).

```python
def delete_duplicates(head: ListNode) -> ListNode:
    """
    Remove duplicates from sorted list.
    
    Time: O(n)
    Space: O(1)
    """
    dummy = ListNode(0, head)
    current = head
    
    while current and current.next:
        if current.val == current.next.val:
            current.next = current.next.next
        else:
            current = current.next
    
    return dummy.next
```

### Pattern 2: Two Pointers (Fast & Slow)

Used for cycle detection, middle element, nth node.

```python
def has_cycle(head: ListNode) -> bool:
    """
    Detect cycle using Floyd's algorithm.
    
    Time: O(n)
    Space: O(1)
    """
    if not head or not head.next:
        return False
    
    slow = head
    fast = head.next
    
    while slow != fast:
        if not fast or not fast.next:
            return False
        slow = slow.next
        fast = fast.next.next
    
    return True

def find_middle(head: ListNode) -> ListNode:
    """
    Find middle node.
    
    Time: O(n)
    Space: O(1)
    """
    slow = fast = head
    
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
    
    return slow
```

### Pattern 3: Reversal

```python
def reverse_list(head: ListNode) -> ListNode:
    """
    Reverse linked list iteratively.
    
    Time: O(n)
    Space: O(1)
    """
    prev = None
    current = head
    
    while current:
        next_node = current.next
        current.next = prev
        prev = current
        current = next_node
    
    return prev

def reverse_list_recursive(head: ListNode) -> ListNode:
    """
    Reverse linked list recursively.
    
    Time: O(n)
    Space: O(n) - recursion stack
    """
    if not head or not head.next:
        return head
    
    new_head = reverse_list_recursive(head.next)
    head.next.next = head
    head.next = None
    
    return new_head
```

### Pattern 4: Runner Technique

Two pointers moving at different speeds.

```python
def remove_nth_from_end(head: ListNode, n: int) -> ListNode:
    """
    Remove nth node from end.
    
    Time: O(L) where L is length
    Space: O(1)
    """
    dummy = ListNode(0, head)
    fast = slow = dummy
    
    # Move fast n+1 steps ahead
    for _ in range(n + 1):
        fast = fast.next
    
    # Move both until fast reaches end
    while fast:
        slow = slow.next
        fast = fast.next
    
    # Remove nth node
    slow.next = slow.next.next
    
    return dummy.head
```

---

## Classic Problems

### 1. Reverse Linked List

```python
def reverse_list(head: ListNode) -> ListNode:
    """
    Time: O(n)
    Space: O(1)
    """
    prev = None
    current = head
    
    while current:
        next_node = current.next
        current.next = prev
        prev = current
        current = next_node
    
    return prev

# Test
# 1 -> 2 -> 3 -> None
# becomes
# 3 -> 2 -> 1 -> None
```

### 2. Merge Two Sorted Lists

```python
def merge_two_lists(l1: ListNode, l2: ListNode) -> ListNode:
    """
    Time: O(n + m)
    Space: O(1)
    """
    dummy = ListNode(0)
    current = dummy
    
    while l1 and l2:
        if l1.val <= l2.val:
            current.next = l1
            l1 = l1.next
        else:
            current.next = l2
            l2 = l2.next
        current = current.next
    
    # Attach remaining nodes
    current.next = l1 if l1 else l2
    
    return dummy.next
```

### 3. Linked List Cycle

```python
def has_cycle(head: ListNode) -> bool:
    """
    Floyd's Cycle Detection (Tortoise and Hare).
    
    Time: O(n)
    Space: O(1)
    """
    if not head or not head.next:
        return False
    
    slow = head
    fast = head.next
    
    while slow != fast:
        if not fast or not fast.next:
            return False
        slow = slow.next
        fast = fast.next.next
    
    return True

def detect_cycle(head: ListNode) -> ListNode:
    """
    Return node where cycle begins.
    
    Time: O(n)
    Space: O(1)
    """
    if not head or not head.next:
        return None
    
    # Detect cycle
    slow = fast = head
    has_cycle = False
    
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        if slow == fast:
            has_cycle = True
            break
    
    if not has_cycle:
        return None
    
    # Find cycle start
    slow = head
    while slow != fast:
        slow = slow.next
        fast = fast.next
    
    return slow
```

### 4. Remove Nth Node From End

```python
def remove_nth_from_end(head: ListNode, n: int) -> ListNode:
    """
    Time: O(L)
    Space: O(1)
    """
    dummy = ListNode(0, head)
    fast = slow = dummy
    
    # Move fast n+1 steps
    for _ in range(n + 1):
        fast = fast.next
    
    # Move both pointers
    while fast:
        slow = slow.next
        fast = fast.next
    
    # Remove nth node
    slow.next = slow.next.next
    
    return dummy.next
```

### 5. Reorder List

**Problem**: Reorder L0→L1→...→Ln-1→Ln to L0→Ln→L1→Ln-1→L2→Ln-2→...

```python
def reorder_list(head: ListNode) -> None:
    """
    Time: O(n)
    Space: O(1)
    """
    if not head or not head.next:
        return
    
    # Find middle
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
    
    # Reverse second half
    prev = None
    current = slow
    while current:
        next_node = current.next
        current.next = prev
        prev = current
        current = next_node
    
    # Merge two halves
    first, second = head, prev
    while second.next:
        tmp1, tmp2 = first.next, second.next
        first.next = second
        second.next = tmp1
        first, second = tmp1, tmp2
```

### 6. Palindrome Linked List

```python
def is_palindrome(head: ListNode) -> bool:
    """
    Time: O(n)
    Space: O(1)
    """
    if not head or not head.next:
        return True
    
    # Find middle
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
    
    # Reverse second half
    prev = None
    while slow:
        next_node = slow.next
        slow.next = prev
        prev = slow
        slow = next_node
    
    # Compare
    left, right = head, prev
    while right:
        if left.val != right.val:
            return False
        left = left.next
        right = right.next
    
    return True
```

### 7. Add Two Numbers

**Problem**: Add two numbers represented by linked lists (reverse order).

```python
def add_two_numbers(l1: ListNode, l2: ListNode) -> ListNode:
    """
    Time: O(max(m, n))
    Space: O(max(m, n))
    """
    dummy = ListNode(0)
    current = dummy
    carry = 0
    
    while l1 or l2 or carry:
        val1 = l1.val if l1 else 0
        val2 = l2.val if l2 else 0
        
        total = val1 + val2 + carry
        carry = total // 10
        
        current.next = ListNode(total % 10)
        current = current.next
        
        if l1:
            l1 = l1.next
        if l2:
            l2 = l2.next
    
    return dummy.next

# Example:
# (2 -> 4 -> 3) + (5 -> 6 -> 4) = (7 -> 0 -> 8)
# 342 + 465 = 807
```

### 8. Intersection of Two Linked Lists

```python
def get_intersection_node(headA: ListNode, headB: ListNode) -> ListNode:
    """
    Time: O(m + n)
    Space: O(1)
    """
    if not headA or not headB:
        return None
    
    # Get lengths
    def get_length(head):
        length = 0
        while head:
            length += 1
            head = head.next
        return length
    
    lenA, lenB = get_length(headA), get_length(headB)
    
    # Align start points
    while lenA > lenB:
        headA = headA.next
        lenA -= 1
    while lenB > lenA:
        headB = headB.next
        lenB -= 1
    
    # Find intersection
    while headA != headB:
        headA = headA.next
        headB = headB.next
    
    return headA

# Alternative: Two pointer technique
def get_intersection_node_v2(headA: ListNode, headB: ListNode) -> ListNode:
    """
    Time: O(m + n)
    Space: O(1)
    """
    if not headA or not headB:
        return None
    
    pA, pB = headA, headB
    
    # When pA reaches end, redirect to headB
    # When pB reaches end, redirect to headA
    # They will meet at intersection (or None)
    while pA != pB:
        pA = pA.next if pA else headB
        pB = pB.next if pB else headA
    
    return pA
```

### 9. Copy List with Random Pointer

```python
class Node:
    def __init__(self, val, next=None, random=None):
        self.val = val
        self.next = next
        self.random = random

def copy_random_list(head: Node) -> Node:
    """
    Time: O(n)
    Space: O(n)
    """
    if not head:
        return None
    
    # Create mapping of old -> new nodes
    old_to_new = {}
    
    # First pass: create all nodes
    current = head
    while current:
        old_to_new[current] = Node(current.val)
        current = current.next
    
    # Second pass: connect pointers
    current = head
    while current:
        if current.next:
            old_to_new[current].next = old_to_new[current.next]
        if current.random:
            old_to_new[current].random = old_to_new[current.random]
        current = current.next
    
    return old_to_new[head]

# Alternative: O(1) space (interleaving)
def copy_random_list_v2(head: Node) -> Node:
    """
    Time: O(n)
    Space: O(1)
    """
    if not head:
        return None
    
    # Step 1: Create copy nodes interleaved with originals
    current = head
    while current:
        copy = Node(current.val)
        copy.next = current.next
        current.next = copy
        current = copy.next
    
    # Step 2: Assign random pointers
    current = head
    while current:
        if current.random:
            current.next.random = current.random.next
        current = current.next.next
    
    # Step 3: Separate lists
    current = head
    new_head = head.next
    while current:
        copy = current.next
        current.next = copy.next
        if copy.next:
            copy.next = copy.next.next
        current = current.next
    
    return new_head
```

### 10. Merge K Sorted Lists

```python
def merge_k_lists(lists: list[ListNode]) -> ListNode:
    """
    Using min heap.
    
    Time: O(N log k) where N is total nodes, k is number of lists
    Space: O(k)
    """
    import heapq
    
    # Min heap of (val, index, node)
    heap = []
    
    # Add first node from each list
    for i, node in enumerate(lists):
        if node:
            heapq.heappush(heap, (node.val, i, node))
    
    dummy = ListNode(0)
    current = dummy
    
    while heap:
        val, i, node = heapq.heappop(heap)
        current.next = node
        current = current.next
        
        # Add next node from same list
        if node.next:
            heapq.heappush(heap, (node.next.val, i, node.next))
    
    return dummy.next

# Alternative: Divide and Conquer
def merge_k_lists_v2(lists: list[ListNode]) -> ListNode:
    """
    Time: O(N log k)
    Space: O(log k) - recursion stack
    """
    if not lists:
        return None
    
    def merge_two(l1, l2):
        dummy = ListNode(0)
        current = dummy
        
        while l1 and l2:
            if l1.val <= l2.val:
                current.next = l1
                l1 = l1.next
            else:
                current.next = l2
                l2 = l2.next
            current = current.next
        
        current.next = l1 if l1 else l2
        return dummy.next
    
    def merge_lists(start, end):
        if start == end:
            return lists[start]
        if start > end:
            return None
        
        mid = (start + end) // 2
        left = merge_lists(start, mid)
        right = merge_lists(mid + 1, end)
        
        return merge_two(left, right)
    
    return merge_lists(0, len(lists) - 1)
```

---

## Interview Tips

### 1. Draw the Problem
Always draw the linked list and visualize pointer movements.

### 2. Handle Edge Cases
```python
# Always check:
- Empty list (head is None)
- Single node
- Two nodes
- Cycle (if applicable)
```

### 3. Use Dummy Head
```python
dummy = ListNode(0)
dummy.next = head
# ... operations ...
return dummy.next
```

### 4. Common Techniques
- **Two pointers**: Fast & slow, runner
- **Dummy node**: Simplifies edge cases
- **Reverse**: Many problems use reversal
- **Hash map**: Track visited nodes

### 5. Space vs Time Trade-offs
```python
# O(1) space: Two pointers, in-place modification
# O(n) space: Hash map, recursion
```

---

## Practice Problems

### Easy
1. [Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)
2. [Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/)
3. [Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/)
4. [Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/)
5. [Remove Linked List Elements](https://leetcode.com/problems/remove-linked-list-elements/)

### Medium
1. [Add Two Numbers](https://leetcode.com/problems/add-two-numbers/)
2. [Remove Nth Node From End](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)
3. [Reorder List](https://leetcode.com/problems/reorder-list/)
4. [Odd Even Linked List](https://leetcode.com/problems/odd-even-linked-list/)
5. [Copy List with Random Pointer](https://leetcode.com/problems/copy-list-with-random-pointer/)

### Hard
1. [Merge K Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/)
2. [Reverse Nodes in K-Group](https://leetcode.com/problems/reverse-nodes-in-k-group/)

---

## Summary

### Key Takeaways
- ✅ Use dummy node for edge cases
- ✅ Two pointers (fast & slow) for many problems
- ✅ Draw diagrams before coding
- ✅ Handle null pointers carefully
- ✅ Practice reversal technique

### Common Patterns
```python
# Dummy head
dummy = ListNode(0, head)

# Two pointers
slow = fast = head
while fast and fast.next:
    slow = slow.next
    fast = fast.next.next

# Reversal
prev = None
while current:
    next_node = current.next
    current.next = prev
    prev = current
    current = next_node
```

---

**Next**: [Stacks →](../04-stacks/README.md)

**Happy Coding! 🚀**
