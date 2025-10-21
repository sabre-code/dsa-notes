# 🔍 Binary Search Trees - Python DSA

> Ordered binary trees with O(log n) operations

---

## 📚 Table of Contents

1. [Introduction](#introduction)
2. [BST Properties](#bst-properties)
3. [Implementation](#implementation)
4. [Common Operations](#common-operations)
5. [Common Patterns](#common-patterns)
6. [Classic Problems](#classic-problems)
7. [Interview Tips](#interview-tips)
8. [Practice Problems](#practice-problems)

---

## Introduction

**Binary Search Tree (BST)** is a binary tree where:
- Left subtree contains only nodes with keys < node's key
- Right subtree contains only nodes with keys > node's key
- Both subtrees are also BSTs

### Key Characteristics
- ✅ **Ordered structure** - Inorder gives sorted sequence
- ✅ **O(log n) operations** - In balanced tree
- ✅ **Binary search** - Efficient lookup
- ✅ **Dynamic insertion** - Easy to add/remove
- ❌ **Can become unbalanced** - Degenerates to O(n)

### BST Operations

| Operation | Average | Worst | Description |
|-----------|---------|-------|-------------|
| Search | O(log n) | O(n) | Find value |
| Insert | O(log n) | O(n) | Add value |
| Delete | O(log n) | O(n) | Remove value |
| Min/Max | O(log n) | O(n) | Find extremes |
| Successor | O(log n) | O(n) | Next larger |

*Worst case when tree is skewed (like linked list)*

---

## BST Properties

### 1. BST Property
For every node:
- All values in left subtree < node value
- All values in right subtree > node value

```
Valid BST:          Invalid BST:
      5                   5
     / \                 / \
    3   7               3   7
   / \   \             / \   \
  1   4   9           1   6   9  ← 6 > 5, invalid!
```

### 2. Inorder Traversal
Inorder traversal of BST gives values in **sorted order**.

```python
def inorder(root):
    if not root:
        return []
    return inorder(root.left) + [root.val] + inorder(root.right)

# BST: [5, 3, 1, 4, 7, 9]
# Inorder: [1, 3, 4, 5, 7, 9]  ← Sorted!
```

### 3. Search Property
Can eliminate half the tree at each step.

```python
def search(root, val):
    if not root or root.val == val:
        return root
    
    if val < root.val:
        return search(root.left, val)  # Go left
    else:
        return search(root.right, val)  # Go right
```

---

## Implementation

### BST Node

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right
```

### BST Class

```python
class BST:
    """Binary Search Tree implementation."""
    
    def __init__(self):
        self.root = None
    
    def insert(self, val):
        """
        Insert value into BST.
        Time: O(log n) average, O(n) worst
        Space: O(log n) recursion stack
        """
        if not self.root:
            self.root = TreeNode(val)
        else:
            self._insert_recursive(self.root, val)
    
    def _insert_recursive(self, node, val):
        """Helper for recursive insertion."""
        if val < node.val:
            if node.left:
                self._insert_recursive(node.left, val)
            else:
                node.left = TreeNode(val)
        else:
            if node.right:
                self._insert_recursive(node.right, val)
            else:
                node.right = TreeNode(val)
    
    def insert_iterative(self, val):
        """
        Iterative insertion.
        Time: O(log n) average
        Space: O(1)
        """
        if not self.root:
            self.root = TreeNode(val)
            return
        
        current = self.root
        while True:
            if val < current.val:
                if current.left:
                    current = current.left
                else:
                    current.left = TreeNode(val)
                    break
            else:
                if current.right:
                    current = current.right
                else:
                    current.right = TreeNode(val)
                    break
    
    def search(self, val):
        """
        Search for value.
        Time: O(log n) average
        Space: O(1)
        """
        current = self.root
        
        while current:
            if val == current.val:
                return True
            elif val < current.val:
                current = current.left
            else:
                current = current.right
        
        return False
    
    def delete(self, val):
        """
        Delete value from BST.
        Time: O(log n) average
        Space: O(log n)
        """
        self.root = self._delete_recursive(self.root, val)
    
    def _delete_recursive(self, node, val):
        """Helper for deletion."""
        if not node:
            return None
        
        if val < node.val:
            node.left = self._delete_recursive(node.left, val)
        elif val > node.val:
            node.right = self._delete_recursive(node.right, val)
        else:
            # Node to delete found
            
            # Case 1: Leaf node
            if not node.left and not node.right:
                return None
            
            # Case 2: One child
            if not node.left:
                return node.right
            if not node.right:
                return node.left
            
            # Case 3: Two children
            # Find inorder successor (min in right subtree)
            successor = self._find_min(node.right)
            node.val = successor.val
            node.right = self._delete_recursive(node.right, successor.val)
        
        return node
    
    def _find_min(self, node):
        """Find minimum node in subtree."""
        while node.left:
            node = node.left
        return node
    
    def _find_max(self, node):
        """Find maximum node in subtree."""
        while node.right:
            node = node.right
        return node
    
    def find_min(self):
        """Find minimum value in tree."""
        if not self.root:
            return None
        return self._find_min(self.root).val
    
    def find_max(self):
        """Find maximum value in tree."""
        if not self.root:
            return None
        return self._find_max(self.root).val
    
    def inorder(self):
        """Return values in sorted order."""
        result = []
        
        def traverse(node):
            if not node:
                return
            traverse(node.left)
            result.append(node.val)
            traverse(node.right)
        
        traverse(self.root)
        return result

# Test
bst = BST()
for val in [5, 3, 7, 1, 4, 6, 9]:
    bst.insert(val)

print(bst.search(4))   # True
print(bst.search(8))   # False
print(bst.inorder())   # [1, 3, 4, 5, 6, 7, 9]
print(bst.find_min())  # 1
print(bst.find_max())  # 9
```

---

## Common Operations

### 1. Find Kth Smallest

```python
def kth_smallest(root: TreeNode, k: int) -> int:
    """
    Find kth smallest element (1-indexed).
    
    Time: O(k)
    Space: O(h)
    """
    count = 0
    result = None
    
    def inorder(node):
        nonlocal count, result
        if not node or result is not None:
            return
        
        inorder(node.left)
        
        count += 1
        if count == k:
            result = node.val
            return
        
        inorder(node.right)
    
    inorder(root)
    return result

# Iterative version
def kth_smallest_iterative(root: TreeNode, k: int) -> int:
    """
    Time: O(k)
    Space: O(h)
    """
    stack = []
    current = root
    count = 0
    
    while current or stack:
        while current:
            stack.append(current)
            current = current.left
        
        current = stack.pop()
        count += 1
        
        if count == k:
            return current.val
        
        current = current.right
    
    return -1
```

### 2. Find Successor/Predecessor

```python
def inorder_successor(root: TreeNode, p: TreeNode) -> TreeNode:
    """
    Find inorder successor of node p.
    
    Time: O(h)
    Space: O(1)
    """
    successor = None
    current = root
    
    while current:
        if p.val < current.val:
            successor = current
            current = current.left
        else:
            current = current.right
    
    return successor

def inorder_predecessor(root: TreeNode, p: TreeNode) -> TreeNode:
    """
    Find inorder predecessor of node p.
    
    Time: O(h)
    Space: O(1)
    """
    predecessor = None
    current = root
    
    while current:
        if p.val > current.val:
            predecessor = current
            current = current.right
        else:
            current = current.left
    
    return predecessor
```

### 3. Range Query

```python
def range_sum_bst(root: TreeNode, low: int, high: int) -> int:
    """
    Sum of values in range [low, high].
    
    Time: O(n)
    Space: O(h)
    """
    if not root:
        return 0
    
    total = 0
    
    # Add current if in range
    if low <= root.val <= high:
        total += root.val
    
    # Search left if possible
    if root.val > low:
        total += range_sum_bst(root.left, low, high)
    
    # Search right if possible
    if root.val < high:
        total += range_sum_bst(root.right, low, high)
    
    return total
```

---

## Common Patterns

### Pattern 1: Validate BST

```python
def is_valid_bst(root: TreeNode) -> bool:
    """
    Check if tree is valid BST.
    
    Time: O(n)
    Space: O(h)
    """
    def validate(node, min_val, max_val):
        if not node:
            return True
        
        # Check current node
        if node.val <= min_val or node.val >= max_val:
            return False
        
        # Validate subtrees with updated bounds
        return (validate(node.left, min_val, node.val) and
                validate(node.right, node.val, max_val))
    
    return validate(root, float('-inf'), float('inf'))

# Alternative: Inorder approach
def is_valid_bst_inorder(root: TreeNode) -> bool:
    """
    Time: O(n)
    Space: O(h)
    """
    prev = float('-inf')
    
    def inorder(node):
        nonlocal prev
        if not node:
            return True
        
        if not inorder(node.left):
            return False
        
        if node.val <= prev:
            return False
        prev = node.val
        
        return inorder(node.right)
    
    return inorder(root)
```

### Pattern 2: BST to Sorted Array

```python
def bst_to_array(root: TreeNode) -> list[int]:
    """
    Convert BST to sorted array.
    
    Time: O(n)
    Space: O(n)
    """
    result = []
    
    def inorder(node):
        if not node:
            return
        inorder(node.left)
        result.append(node.val)
        inorder(node.right)
    
    inorder(root)
    return result
```

### Pattern 3: Array to Balanced BST

```python
def sorted_array_to_bst(nums: list[int]) -> TreeNode:
    """
    Convert sorted array to balanced BST.
    
    Time: O(n)
    Space: O(log n)
    """
    if not nums:
        return None
    
    mid = len(nums) // 2
    root = TreeNode(nums[mid])
    
    root.left = sorted_array_to_bst(nums[:mid])
    root.right = sorted_array_to_bst(nums[mid+1:])
    
    return root

# Optimized without slicing
def sorted_array_to_bst_optimized(nums: list[int]) -> TreeNode:
    """
    Time: O(n)
    Space: O(log n)
    """
    def build(left, right):
        if left > right:
            return None
        
        mid = (left + right) // 2
        root = TreeNode(nums[mid])
        
        root.left = build(left, mid - 1)
        root.right = build(mid + 1, right)
        
        return root
    
    return build(0, len(nums) - 1)
```

---

## Classic Problems

### 1. Validate Binary Search Tree

```python
def is_valid_bst(root: TreeNode) -> bool:
    """
    Time: O(n)
    Space: O(h)
    """
    def validate(node, min_val, max_val):
        if not node:
            return True
        
        if node.val <= min_val or node.val >= max_val:
            return False
        
        return (validate(node.left, min_val, node.val) and
                validate(node.right, node.val, max_val))
    
    return validate(root, float('-inf'), float('inf'))
```

### 2. Search in BST

```python
def search_bst(root: TreeNode, val: int) -> TreeNode:
    """
    Time: O(h)
    Space: O(1)
    """
    current = root
    
    while current:
        if val == current.val:
            return current
        elif val < current.val:
            current = current.left
        else:
            current = current.right
    
    return None
```

### 3. Insert into BST

```python
def insert_into_bst(root: TreeNode, val: int) -> TreeNode:
    """
    Time: O(h)
    Space: O(h)
    """
    if not root:
        return TreeNode(val)
    
    if val < root.val:
        root.left = insert_into_bst(root.left, val)
    else:
        root.right = insert_into_bst(root.right, val)
    
    return root
```

### 4. Delete Node in BST

```python
def delete_node(root: TreeNode, key: int) -> TreeNode:
    """
    Time: O(h)
    Space: O(h)
    """
    if not root:
        return None
    
    if key < root.val:
        root.left = delete_node(root.left, key)
    elif key > root.val:
        root.right = delete_node(root.right, key)
    else:
        # Node to delete found
        
        # Case 1 & 2: Leaf or one child
        if not root.left:
            return root.right
        if not root.right:
            return root.left
        
        # Case 3: Two children
        # Find successor (min in right subtree)
        successor = root.right
        while successor.left:
            successor = successor.left
        
        root.val = successor.val
        root.right = delete_node(root.right, successor.val)
    
    return root
```

### 5. Lowest Common Ancestor of BST

```python
def lowest_common_ancestor_bst(root: TreeNode, p: TreeNode, q: TreeNode) -> TreeNode:
    """
    Time: O(h)
    Space: O(1)
    """
    current = root
    
    while current:
        # Both in left subtree
        if p.val < current.val and q.val < current.val:
            current = current.left
        # Both in right subtree
        elif p.val > current.val and q.val > current.val:
            current = current.right
        # Split or one equals current
        else:
            return current
    
    return None
```

### 6. Kth Smallest Element

```python
def kth_smallest(root: TreeNode, k: int) -> int:
    """
    Time: O(h + k)
    Space: O(h)
    """
    stack = []
    current = root
    count = 0
    
    while current or stack:
        while current:
            stack.append(current)
            current = current.left
        
        current = stack.pop()
        count += 1
        
        if count == k:
            return current.val
        
        current = current.right
    
    return -1
```

### 7. Convert Sorted Array to BST

```python
def sorted_array_to_bst(nums: list[int]) -> TreeNode:
    """
    Time: O(n)
    Space: O(log n)
    """
    def build(left, right):
        if left > right:
            return None
        
        mid = (left + right) // 2
        root = TreeNode(nums[mid])
        
        root.left = build(left, mid - 1)
        root.right = build(mid + 1, right)
        
        return root
    
    return build(0, len(nums) - 1)
```

### 8. Recover BST

```python
def recover_tree(root: TreeNode) -> None:
    """
    Two nodes swapped by mistake. Fix the BST.
    
    Time: O(n)
    Space: O(h)
    """
    first = second = prev = None
    
    def inorder(node):
        nonlocal first, second, prev
        
        if not node:
            return
        
        inorder(node.left)
        
        # Find violations
        if prev and node.val < prev.val:
            if not first:
                first = prev
            second = node
        
        prev = node
        inorder(node.right)
    
    inorder(root)
    
    # Swap values
    if first and second:
        first.val, second.val = second.val, first.val
```

### 9. BST Iterator

```python
class BSTIterator:
    """
    Iterator for BST in-order traversal.
    """
    
    def __init__(self, root: TreeNode):
        """
        Time: O(h)
        Space: O(h)
        """
        self.stack = []
        self._push_left(root)
    
    def _push_left(self, node):
        """Push all left nodes."""
        while node:
            self.stack.append(node)
            node = node.left
    
    def next(self) -> int:
        """
        Time: O(1) amortized
        """
        node = self.stack.pop()
        self._push_left(node.right)
        return node.val
    
    def has_next(self) -> bool:
        """
        Time: O(1)
        """
        return len(self.stack) > 0

# Test
# tree = [7, 3, 15, null, null, 9, 20]
iterator = BSTIterator(root)
print(iterator.next())      # 3
print(iterator.next())      # 7
print(iterator.has_next())  # True
print(iterator.next())      # 9
```

### 10. Trim BST

```python
def trim_bst(root: TreeNode, low: int, high: int) -> TreeNode:
    """
    Remove nodes outside range [low, high].
    
    Time: O(n)
    Space: O(h)
    """
    if not root:
        return None
    
    # Current node too small
    if root.val < low:
        return trim_bst(root.right, low, high)
    
    # Current node too large
    if root.val > high:
        return trim_bst(root.left, low, high)
    
    # Current node in range
    root.left = trim_bst(root.left, low, high)
    root.right = trim_bst(root.right, low, high)
    
    return root
```

### 11. Two Sum IV - BST

```python
def find_target(root: TreeNode, k: int) -> bool:
    """
    Find if two elements sum to k.
    
    Time: O(n)
    Space: O(n)
    """
    seen = set()
    
    def dfs(node):
        if not node:
            return False
        
        if k - node.val in seen:
            return True
        
        seen.add(node.val)
        
        return dfs(node.left) or dfs(node.right)
    
    return dfs(root)

# Alternative: Two pointers with inorder
def find_target_two_pointers(root: TreeNode, k: int) -> bool:
    """
    Time: O(n)
    Space: O(n)
    """
    # Get sorted array via inorder
    nums = []
    
    def inorder(node):
        if not node:
            return
        inorder(node.left)
        nums.append(node.val)
        inorder(node.right)
    
    inorder(root)
    
    # Two pointers
    left, right = 0, len(nums) - 1
    
    while left < right:
        total = nums[left] + nums[right]
        if total == k:
            return True
        elif total < k:
            left += 1
        else:
            right -= 1
    
    return False
```

### 12. Balance BST

```python
def balance_bst(root: TreeNode) -> TreeNode:
    """
    Convert BST to balanced BST.
    
    Time: O(n)
    Space: O(n)
    """
    # Step 1: Get sorted values
    values = []
    
    def inorder(node):
        if not node:
            return
        inorder(node.left)
        values.append(node.val)
        inorder(node.right)
    
    inorder(root)
    
    # Step 2: Build balanced BST
    def build(left, right):
        if left > right:
            return None
        
        mid = (left + right) // 2
        node = TreeNode(values[mid])
        
        node.left = build(left, mid - 1)
        node.right = build(mid + 1, right)
        
        return node
    
    return build(0, len(values) - 1)
```

---

## Interview Tips

### 1. BST vs Binary Tree

```python
# Binary Tree: No ordering constraint
# BST: Left < Root < Right (ordered)

# Use BST property to optimize:
def search_bst(root, val):
    if not root:
        return None
    if val < root.val:
        return search_bst(root.left, val)  # Only left
    elif val > root.val:
        return search_bst(root.right, val)  # Only right
    return root
```

### 2. Inorder = Sorted

```python
# For BST, inorder traversal gives sorted sequence
def validate_bst(root):
    prev = float('-inf')
    
    def inorder(node):
        nonlocal prev
        if not node:
            return True
        
        if not inorder(node.left):
            return False
        
        if node.val <= prev:  # Check sorted
            return False
        prev = node.val
        
        return inorder(node.right)
    
    return inorder(root)
```

### 3. Range Bounds Pattern

```python
# Keep track of valid range for each node
def validate(node, min_val, max_val):
    if not node:
        return True
    
    if node.val <= min_val or node.val >= max_val:
        return False
    
    return (validate(node.left, min_val, node.val) and
            validate(node.right, node.val, max_val))
```

### 4. Common BST Operations

```python
# Find min: Go all the way left
def find_min(root):
    while root.left:
        root = root.left
    return root

# Find max: Go all the way right
def find_max(root):
    while root.right:
        root = root.right
    return root

# Successor: Smallest node > current
# If has right child: min of right subtree
# Otherwise: ancestor where we went left
```

---

## Practice Problems

### Easy
1. [Search in BST](https://leetcode.com/problems/search-in-a-binary-search-tree/)
2. [Insert into BST](https://leetcode.com/problems/insert-into-a-binary-search-tree/)
3. [Range Sum of BST](https://leetcode.com/problems/range-sum-of-bst/)
4. [Minimum Distance Between BST Nodes](https://leetcode.com/problems/minimum-distance-between-bst-nodes/)
5. [Two Sum IV - BST](https://leetcode.com/problems/two-sum-iv-input-is-a-bst/)
6. [Convert Sorted Array to BST](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/)

### Medium
1. [Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/)
2. [Kth Smallest Element](https://leetcode.com/problems/kth-smallest-element-in-a-bst/)
3. [Delete Node in BST](https://leetcode.com/problems/delete-node-in-a-bst/)
4. [Lowest Common Ancestor of BST](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)
5. [Trim a BST](https://leetcode.com/problems/trim-a-binary-search-tree/)
6. [Balance BST](https://leetcode.com/problems/balance-a-binary-search-tree/)
7. [BST Iterator](https://leetcode.com/problems/binary-search-tree-iterator/)

### Hard
1. [Recover Binary Search Tree](https://leetcode.com/problems/recover-binary-search-tree/)
2. [Count of Smaller Numbers After Self](https://leetcode.com/problems/count-of-smaller-numbers-after-self/)

---

## Summary

### Key Takeaways
- ✅ BST: Left < Root < Right property
- ✅ Inorder traversal gives sorted order
- ✅ O(log n) operations in balanced tree
- ✅ Use range bounds for validation
- ✅ Can degenerate to O(n) if unbalanced

### Common Patterns
```python
# 1. BST search (binary search)
def search(root, val):
    if not root:
        return None
    if val < root.val:
        return search(root.left, val)
    elif val > root.val:
        return search(root.right, val)
    return root

# 2. Validate BST
def validate(node, min_val, max_val):
    if not node:
        return True
    if node.val <= min_val or node.val >= max_val:
        return False
    return (validate(node.left, min_val, node.val) and
            validate(node.right, node.val, max_val))

# 3. Inorder for sorted values
def inorder(root):
    if not root:
        return []
    return inorder(root.left) + [root.val] + inorder(root.right)

# 4. Build balanced BST
def build(nums, left, right):
    if left > right:
        return None
    mid = (left + right) // 2
    root = TreeNode(nums[mid])
    root.left = build(nums, left, mid - 1)
    root.right = build(nums, mid + 1, right)
    return root
```

---

**Next**: [Heaps & Priority Queues →](../09-heaps/README.md)

**Happy Coding! 🚀**
