# 🐢🐇 Fast & Slow Pointers - Python DSA

> Detect cycles and find middle elements efficiently

---

## 📚 Table of Contents

1. [Introduction](#introduction)
2. [Pattern Recognition](#pattern-recognition)
3. [Linked List Problems](#linked-list-problems)
4. [Array Problems](#array-problems)
5. [Interview Tips](#interview-tips)
6. [Practice Problems](#practice-problems)

---

## Introduction

**Fast & Slow Pointers** (Floyd's Cycle Detection) uses two pointers moving at different speeds.

### Key Characteristics
- ✅ **Two pointers** - One fast (2x speed), one slow (1x speed)
- ✅ **Cycle detection** - Pointers meet if cycle exists
- ✅ **O(1) space** - No extra data structures needed
- ✅ **Finding middle** - Slow pointer at middle when fast at end

### When to Use
- Detect **cycles** in linked list
- Find **middle** of linked list
- Find **duplicate** in array (cycle mapping)
- Keywords: "cycle", "middle", "tortoise and hare"

---

## Pattern Recognition

### Floyd's Cycle Detection Algorithm

```python
# Basic Pattern:
slow = fast = head

while fast and fast.next:
    slow = slow.next        # Move 1 step
    fast = fast.next.next   # Move 2 steps
    
    if slow == fast:
        # Cycle detected
        break
```

### Why It Works

```python
# If cycle exists:
# - Fast pointer enters cycle first
# - Slow pointer enters cycle later
# - Fast gains 1 position per iteration
# - They must eventually meet

# Mathematical proof:
# Let cycle length = C
# When slow enters cycle, fast is k nodes ahead
# Distance to close = C - k
# Time to meet = C - k iterations (fast gains 1 per iteration)
```

---

## Linked List Problems

### Problem 1: Linked List Cycle

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def has_cycle(head: ListNode) -> bool:
    """
    Detect if linked list has cycle.
    
    Time: O(n)
    Space: O(1)
    
    Example: head = [3,2,0,-4], pos = 1 (cycle at node 1)
    Output: True
    """
    if not head or not head.next:
        return False
    
    slow = fast = head
    
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        
        if slow == fast:
            return True
    
    return False
```

### Problem 2: Linked List Cycle II (Find Start)

```python
def detect_cycle(head: ListNode) -> ListNode:
    """
    Find start of cycle.
    
    Time: O(n)
    Space: O(1)
    
    Example: head = [3,2,0,-4], pos = 1
    Output: Node with value 2
    
    Algorithm:
    1. Find meeting point using fast & slow
    2. Reset one pointer to head
    3. Move both at same speed
    4. They meet at cycle start
    """
    if not head or not head.next:
        return None
    
    # Find meeting point
    slow = fast = head
    
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        
        if slow == fast:
            break
    else:
        return None  # No cycle
    
    # Find cycle start
    slow = head
    while slow != fast:
        slow = slow.next
        fast = fast.next
    
    return slow
```

### Problem 3: Middle of Linked List

```python
def middle_node(head: ListNode) -> ListNode:
    """
    Find middle node of linked list.
    
    Time: O(n)
    Space: O(1)
    
    Example: head = [1,2,3,4,5]
    Output: [3,4,5] (node with value 3)
    
    If even length, return second middle node.
    """
    slow = fast = head
    
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
    
    return slow
```

### Problem 4: Palindrome Linked List

```python
def is_palindrome(head: ListNode) -> bool:
    """
    Check if linked list is palindrome.
    
    Time: O(n)
    Space: O(1)
    
    Example: head = [1,2,2,1]
    Output: True
    
    Algorithm:
    1. Find middle using fast & slow
    2. Reverse second half
    3. Compare both halves
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
    
    # Compare both halves
    left, right = head, prev
    while right:
        if left.val != right.val:
            return False
        left = left.next
        right = right.next
    
    return True
```

### Problem 5: Reorder List

```python
def reorder_list(head: ListNode) -> None:
    """
    Reorder list: L0→L1→...→Ln-1→Ln to L0→Ln→L1→Ln-1→L2→Ln-2→...
    
    Time: O(n)
    Space: O(1)
    
    Example: head = [1,2,3,4]
    Output: [1,4,2,3]
    
    Modifies list in-place.
    """
    if not head or not head.next:
        return
    
    # Find middle
    slow = fast = head
    while fast.next and fast.next.next:
        slow = slow.next
        fast = fast.next.next
    
    # Reverse second half
    second = slow.next
    slow.next = None
    prev = None
    
    while second:
        next_node = second.next
        second.next = prev
        prev = second
        second = next_node
    
    # Merge two halves
    first, second = head, prev
    while second:
        next1, next2 = first.next, second.next
        first.next = second
        second.next = next1
        first, second = next1, next2
```

### Problem 6: Remove Nth Node From End

```python
def remove_nth_from_end(head: ListNode, n: int) -> ListNode:
    """
    Remove nth node from end of list.
    
    Time: O(n)
    Space: O(1)
    
    Example: head = [1,2,3,4,5], n = 2
    Output: [1,2,3,5]
    
    Algorithm: Fast pointer n steps ahead, then move both.
    """
    dummy = ListNode(0)
    dummy.next = head
    slow = fast = dummy
    
    # Move fast n+1 steps ahead
    for _ in range(n + 1):
        fast = fast.next
    
    # Move both until fast reaches end
    while fast:
        slow = slow.next
        fast = fast.next
    
    # Remove node
    slow.next = slow.next.next
    
    return dummy.next
```

### Problem 7: Happy Number

```python
def is_happy(n: int) -> bool:
    """
    Check if number is happy (cycle detection).
    
    Time: O(log n)
    Space: O(1)
    
    Example: n = 19
    Output: True (1²+9²=82, 8²+2²=68, ..., eventually 1)
    
    Algorithm: Use fast & slow pointers to detect cycle.
    """
    def get_next(num):
        total = 0
        while num > 0:
            digit = num % 10
            total += digit * digit
            num //= 10
        return total
    
    slow = fast = n
    
    while True:
        slow = get_next(slow)
        fast = get_next(get_next(fast))
        
        if fast == 1:
            return True
        
        if slow == fast:
            return False
```

---

## Array Problems

### Problem 8: Find Duplicate Number

```python
def find_duplicate(nums: list[int]) -> int:
    """
    Find duplicate in array (cycle detection).
    
    Time: O(n)
    Space: O(1)
    
    Example: nums = [1,3,4,2,2]
    Output: 2
    
    Constraint: Array contains n+1 integers in range [1,n]
    
    Algorithm: Treat array as linked list (value as next index)
    """
    # Find meeting point
    slow = fast = nums[0]
    
    while True:
        slow = nums[slow]
        fast = nums[nums[fast]]
        
        if slow == fast:
            break
    
    # Find duplicate (cycle start)
    slow = nums[0]
    while slow != fast:
        slow = nums[slow]
        fast = nums[fast]
    
    return slow
```

### Problem 9: Circular Array Loop

```python
def circular_array_loop(nums: list[int]) -> bool:
    """
    Check if circular array has cycle (all same direction).
    
    Time: O(n)
    Space: O(1)
    
    Example: nums = [2,-1,1,2,2]
    Output: True
    """
    n = len(nums)
    
    def get_next(i):
        return (i + nums[i]) % n
    
    for i in range(n):
        if nums[i] == 0:
            continue
        
        slow = fast = i
        
        # Check if all moves in same direction
        while (nums[get_next(slow)] * nums[slow] > 0 and
               nums[get_next(get_next(fast))] * nums[get_next(fast)] > 0 and
               nums[get_next(get_next(fast))] * nums[fast] > 0):
            
            slow = get_next(slow)
            fast = get_next(get_next(fast))
            
            if slow == fast:
                # Check if cycle length > 1
                if slow == get_next(slow):
                    break
                return True
        
        # Mark visited
        slow = i
        while nums[slow] * nums[i] > 0:
            next_idx = get_next(slow)
            nums[slow] = 0
            slow = next_idx
    
    return False
```

---

## Interview Tips

### 1. Fast & Slow Pointer Template

```python
# Template for Cycle Detection:
slow = fast = head

while fast and fast.next:
    slow = slow.next
    fast = fast.next.next
    
    if slow == fast:
        # Cycle detected
        break

# Template for Finding Middle:
slow = fast = head

while fast and fast.next:
    slow = slow.next
    fast = fast.next.next

# slow is now at middle
```

### 2. Finding Cycle Start

```python
# After detecting cycle:
# 1. Reset slow to head
# 2. Move both at same speed
# 3. They meet at cycle start

slow = head
while slow != fast:
    slow = slow.next
    fast = fast.next

return slow  # Cycle start
```

### 3. Common Patterns

```python
# Pattern 1: Cycle Detection (Linked List)
def has_cycle(head):
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        if slow == fast:
            return True
    return False

# Pattern 2: Find Middle
def find_middle(head):
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
    return slow

# Pattern 3: Array as Linked List
def find_duplicate(nums):
    slow = fast = nums[0]
    while True:
        slow = nums[slow]
        fast = nums[nums[fast]]
        if slow == fast:
            break
    slow = nums[0]
    while slow != fast:
        slow = nums[slow]
        fast = nums[fast]
    return slow
```

### 4. Edge Cases

```python
# Check for empty list
if not head or not head.next:
    return result

# Check for fast.next before fast.next.next
while fast and fast.next:
    # Safe to access fast.next.next

# Handle single element
if head.next is None:
    return head
```

---

## Practice Problems

### Easy
1. [Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/)
2. [Middle of the Linked List](https://leetcode.com/problems/middle-of-the-linked-list/)
3. [Happy Number](https://leetcode.com/problems/happy-number/)
4. [Remove Linked List Elements](https://leetcode.com/problems/remove-linked-list-elements/)

### Medium
1. [Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/)
2. [Find the Duplicate Number](https://leetcode.com/problems/find-the-duplicate-number/)
3. [Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/)
4. [Reorder List](https://leetcode.com/problems/reorder-list/)
5. [Remove Nth Node From End](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)
6. [Circular Array Loop](https://leetcode.com/problems/circular-array-loop/)

### Hard
1. [Reverse Nodes in k-Group](https://leetcode.com/problems/reverse-nodes-in-k-group/)

---

## Summary

### Key Takeaways
- ✅ **Two speeds** - Fast (2x), slow (1x)
- ✅ **Cycle detection** - Pointers meet if cycle exists
- ✅ **O(1) space** - No extra data structures
- ✅ **Multiple uses** - Cycles, middle, palindrome

### Quick Reference

```python
# Cycle Detection
slow = fast = head
while fast and fast.next:
    slow = slow.next
    fast = fast.next.next
    if slow == fast:
        return True
return False

# Find Middle
slow = fast = head
while fast and fast.next:
    slow = slow.next
    fast = fast.next.next
return slow

# Find Cycle Start
# After detecting cycle:
slow = head
while slow != fast:
    slow = slow.next
    fast = fast.next
return slow
```

---

**Next**: [Intervals →](../10-intervals/README.md)

**Happy Coding! 🚀**
