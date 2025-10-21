# 🌳 Binary Trees - Python DSA

> Hierarchical data structure with powerful traversal patterns

---

## 📚 Table of Contents

1. [Introduction](#introduction)
2. [Tree Terminology](#tree-terminology)
3. [Types of Binary Trees](#types-of-binary-trees)
4. [Implementation](#implementation)
5. [Tree Traversals](#tree-traversals)
6. [Common Patterns](#common-patterns)
7. [Classic Problems](#classic-problems)
8. [Interview Tips](#interview-tips)
9. [Practice Problems](#practice-problems)

---

## Introduction

**Binary Tree** is a hierarchical data structure where each node has at most two children (left and right).

### Key Characteristics
- ✅ **Hierarchical structure** - Parent-child relationships
- ✅ **Recursive nature** - Subtrees are also binary trees
- ✅ **Efficient operations** - Many O(log n) operations
- ✅ **Multiple traversals** - Different ways to visit nodes
- ❌ **No guaranteed balance** - Can degenerate to linked list

### Real-World Examples
- 📁 File system hierarchy
- 🌐 DOM tree in HTML
- 🧬 Expression trees
- 🎮 Game decision trees
- 🔍 Binary search trees (BST)

### Tree Operations

| Operation | Average | Worst | Description |
|-----------|---------|-------|-------------|
| Search | O(log n) | O(n) | Find node |
| Insert | O(log n) | O(n) | Add node |
| Delete | O(log n) | O(n) | Remove node |
| Traversal | O(n) | O(n) | Visit all nodes |

*Depends on tree balance*

---

## Tree Terminology

```
         1          ← Root
       /   \
      2     3       ← Internal nodes
     / \   /
    4   5 6         ← Leaf nodes

Height: 2 (root to farthest leaf)
Depth of node 4: 2 (root to node)
Level: 0 (root), 1 (nodes 2,3), 2 (nodes 4,5,6)
```

### Key Terms
- **Root**: Top node (no parent)
- **Leaf**: Node with no children
- **Internal node**: Node with at least one child
- **Height**: Longest path from node to leaf
- **Depth**: Path length from root to node
- **Level**: All nodes at same depth
- **Subtree**: Tree formed by node and descendants
- **Ancestor**: Node on path from root to given node
- **Descendant**: Node on path from given node to leaf

---

## Types of Binary Trees

### 1. Full Binary Tree
Every node has 0 or 2 children.

```
       1
      / \
     2   3
    / \
   4   5
```

### 2. Complete Binary Tree
All levels filled except possibly last, filled left to right.

```
       1
      / \
     2   3
    / \  /
   4  5 6
```

### 3. Perfect Binary Tree
All internal nodes have 2 children, all leaves at same level.

```
       1
      / \
     2   3
    / \ / \
   4  5 6  7
```

### 4. Balanced Binary Tree
Height difference between left and right subtrees ≤ 1 for all nodes.

```
       1
      / \
     2   3
    /
   4
```

### 5. Degenerate Tree
Each node has only one child (like linked list).

```
1
 \
  2
   \
    3
```

---

## Implementation

### TreeNode Class

```python
class TreeNode:
    """Node for binary tree."""
    
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right
    
    def __repr__(self):
        return f"TreeNode({self.val})"

# Create tree:
#     1
#    / \
#   2   3
root = TreeNode(1)
root.left = TreeNode(2)
root.right = TreeNode(3)
```

### Binary Tree Class

```python
class BinaryTree:
    """Binary tree with common operations."""
    
    def __init__(self, root=None):
        self.root = root
    
    def insert_level_order(self, values: list):
        """Insert values in level order."""
        if not values:
            return
        
        self.root = TreeNode(values[0])
        queue = [self.root]
        i = 1
        
        while queue and i < len(values):
            node = queue.pop(0)
            
            # Left child
            if i < len(values) and values[i] is not None:
                node.left = TreeNode(values[i])
                queue.append(node.left)
            i += 1
            
            # Right child
            if i < len(values) and values[i] is not None:
                node.right = TreeNode(values[i])
                queue.append(node.right)
            i += 1
    
    def height(self, node=None):
        """Get height of tree."""
        if node is None:
            node = self.root
        
        if node is None:
            return -1
        
        left_height = self.height(node.left)
        right_height = self.height(node.right)
        
        return 1 + max(left_height, right_height)
    
    def size(self, node=None):
        """Count number of nodes."""
        if node is None:
            node = self.root
        
        if node is None:
            return 0
        
        return 1 + self.size(node.left) + self.size(node.right)
    
    def is_balanced(self, node=None):
        """Check if tree is balanced."""
        if node is None:
            node = self.root
        
        def check_height(node):
            if not node:
                return 0
            
            left = check_height(node.left)
            if left == -1:
                return -1
            
            right = check_height(node.right)
            if right == -1:
                return -1
            
            if abs(left - right) > 1:
                return -1
            
            return 1 + max(left, right)
        
        return check_height(node) != -1

# Test
tree = BinaryTree()
tree.insert_level_order([1, 2, 3, 4, 5])
print(tree.height())  # 2
print(tree.size())    # 5
```

---

## Tree Traversals

### 1. Depth-First Search (DFS)

#### Inorder (Left → Root → Right)

```python
def inorder_recursive(root: TreeNode) -> list:
    """
    Inorder: Left, Root, Right
    For BST: gives sorted order
    
    Time: O(n)
    Space: O(h) where h is height
    """
    result = []
    
    def traverse(node):
        if not node:
            return
        traverse(node.left)
        result.append(node.val)
        traverse(node.right)
    
    traverse(root)
    return result

def inorder_iterative(root: TreeNode) -> list:
    """
    Inorder using stack.
    
    Time: O(n)
    Space: O(h)
    """
    result = []
    stack = []
    current = root
    
    while current or stack:
        # Go to leftmost node
        while current:
            stack.append(current)
            current = current.left
        
        # Process node
        current = stack.pop()
        result.append(current.val)
        
        # Move to right subtree
        current = current.right
    
    return result
```

#### Preorder (Root → Left → Right)

```python
def preorder_recursive(root: TreeNode) -> list:
    """
    Preorder: Root, Left, Right
    Used for: tree copying, prefix expression
    
    Time: O(n)
    Space: O(h)
    """
    result = []
    
    def traverse(node):
        if not node:
            return
        result.append(node.val)
        traverse(node.left)
        traverse(node.right)
    
    traverse(root)
    return result

def preorder_iterative(root: TreeNode) -> list:
    """
    Preorder using stack.
    
    Time: O(n)
    Space: O(h)
    """
    if not root:
        return []
    
    result = []
    stack = [root]
    
    while stack:
        node = stack.pop()
        result.append(node.val)
        
        # Push right first (so left is processed first)
        if node.right:
            stack.append(node.right)
        if node.left:
            stack.append(node.left)
    
    return result
```

#### Postorder (Left → Right → Root)

```python
def postorder_recursive(root: TreeNode) -> list:
    """
    Postorder: Left, Right, Root
    Used for: deletion, postfix expression
    
    Time: O(n)
    Space: O(h)
    """
    result = []
    
    def traverse(node):
        if not node:
            return
        traverse(node.left)
        traverse(node.right)
        result.append(node.val)
    
    traverse(root)
    return result

def postorder_iterative(root: TreeNode) -> list:
    """
    Postorder using two stacks.
    
    Time: O(n)
    Space: O(h)
    """
    if not root:
        return []
    
    stack1 = [root]
    stack2 = []
    
    while stack1:
        node = stack1.pop()
        stack2.append(node)
        
        if node.left:
            stack1.append(node.left)
        if node.right:
            stack1.append(node.right)
    
    result = []
    while stack2:
        result.append(stack2.pop().val)
    
    return result
```

### 2. Breadth-First Search (BFS)

#### Level Order

```python
from collections import deque

def level_order(root: TreeNode) -> list[list[int]]:
    """
    Level order traversal (BFS).
    
    Time: O(n)
    Space: O(w) where w is max width
    """
    if not root:
        return []
    
    result = []
    queue = deque([root])
    
    while queue:
        level_size = len(queue)
        level = []
        
        for _ in range(level_size):
            node = queue.popleft()
            level.append(node.val)
            
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)
        
        result.append(level)
    
    return result

# Example:
#     3
#    / \
#   9  20
#     /  \
#    15   7
# Output: [[3], [9, 20], [15, 7]]
```

---

## Common Patterns

### Pattern 1: Recursive DFS Template

```python
def dfs_template(root: TreeNode):
    """
    General DFS template.
    
    Time: O(n)
    Space: O(h)
    """
    # Base case
    if not root:
        return base_value
    
    # Recursive calls
    left_result = dfs_template(root.left)
    right_result = dfs_template(root.right)
    
    # Combine results
    return combine(root.val, left_result, right_result)
```

### Pattern 2: Level Order BFS Template

```python
from collections import deque

def bfs_template(root: TreeNode):
    """
    General BFS template.
    
    Time: O(n)
    Space: O(w)
    """
    if not root:
        return []
    
    result = []
    queue = deque([root])
    
    while queue:
        level_size = len(queue)
        level = []
        
        for _ in range(level_size):
            node = queue.popleft()
            
            # Process node
            level.append(node.val)
            
            # Add children
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)
        
        result.append(level)
    
    return result
```

### Pattern 3: Path Finding

```python
def has_path_sum(root: TreeNode, target: int) -> bool:
    """
    Check if path from root to leaf sums to target.
    
    Time: O(n)
    Space: O(h)
    """
    if not root:
        return False
    
    # Leaf node
    if not root.left and not root.right:
        return root.val == target
    
    # Check left and right paths
    remaining = target - root.val
    return (has_path_sum(root.left, remaining) or 
            has_path_sum(root.right, remaining))
```

---

## Classic Problems

### 1. Maximum Depth

```python
def max_depth(root: TreeNode) -> int:
    """
    Time: O(n)
    Space: O(h)
    """
    if not root:
        return 0
    
    left_depth = max_depth(root.left)
    right_depth = max_depth(root.right)
    
    return 1 + max(left_depth, right_depth)

# Iterative BFS
def max_depth_bfs(root: TreeNode) -> int:
    """
    Time: O(n)
    Space: O(w)
    """
    if not root:
        return 0
    
    from collections import deque
    queue = deque([root])
    depth = 0
    
    while queue:
        depth += 1
        level_size = len(queue)
        
        for _ in range(level_size):
            node = queue.popleft()
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)
    
    return depth
```

### 2. Same Tree

```python
def is_same_tree(p: TreeNode, q: TreeNode) -> bool:
    """
    Time: O(n)
    Space: O(h)
    """
    # Both None
    if not p and not q:
        return True
    
    # One None, other not
    if not p or not q:
        return False
    
    # Values different
    if p.val != q.val:
        return False
    
    # Check left and right subtrees
    return (is_same_tree(p.left, q.left) and 
            is_same_tree(p.right, q.right))
```

### 3. Invert Binary Tree

```python
def invert_tree(root: TreeNode) -> TreeNode:
    """
    Time: O(n)
    Space: O(h)
    """
    if not root:
        return None
    
    # Swap children
    root.left, root.right = root.right, root.left
    
    # Recursively invert subtrees
    invert_tree(root.left)
    invert_tree(root.right)
    
    return root
```

### 4. Symmetric Tree

```python
def is_symmetric(root: TreeNode) -> bool:
    """
    Time: O(n)
    Space: O(h)
    """
    def is_mirror(left, right):
        if not left and not right:
            return True
        if not left or not right:
            return False
        
        return (left.val == right.val and
                is_mirror(left.left, right.right) and
                is_mirror(left.right, right.left))
    
    return is_mirror(root, root)
```

### 5. Diameter of Binary Tree

```python
def diameter_of_binary_tree(root: TreeNode) -> int:
    """
    Longest path between any two nodes.
    
    Time: O(n)
    Space: O(h)
    """
    diameter = 0
    
    def height(node):
        nonlocal diameter
        
        if not node:
            return 0
        
        left = height(node.left)
        right = height(node.right)
        
        # Update diameter
        diameter = max(diameter, left + right)
        
        return 1 + max(left, right)
    
    height(root)
    return diameter
```

### 6. Balanced Binary Tree

```python
def is_balanced(root: TreeNode) -> bool:
    """
    Time: O(n)
    Space: O(h)
    """
    def check_height(node):
        if not node:
            return 0
        
        left = check_height(node.left)
        if left == -1:
            return -1
        
        right = check_height(node.right)
        if right == -1:
            return -1
        
        if abs(left - right) > 1:
            return -1
        
        return 1 + max(left, right)
    
    return check_height(root) != -1
```

### 7. Path Sum

```python
def has_path_sum(root: TreeNode, target_sum: int) -> bool:
    """
    Time: O(n)
    Space: O(h)
    """
    if not root:
        return False
    
    # Leaf node
    if not root.left and not root.right:
        return root.val == target_sum
    
    remaining = target_sum - root.val
    return (has_path_sum(root.left, remaining) or
            has_path_sum(root.right, remaining))

def path_sum_all_paths(root: TreeNode, target_sum: int) -> list[list[int]]:
    """
    Find all root-to-leaf paths with sum.
    
    Time: O(n²) - copying paths
    Space: O(h)
    """
    result = []
    
    def dfs(node, remaining, path):
        if not node:
            return
        
        path.append(node.val)
        
        # Leaf node
        if not node.left and not node.right and remaining == node.val:
            result.append(path[:])
        else:
            dfs(node.left, remaining - node.val, path)
            dfs(node.right, remaining - node.val, path)
        
        path.pop()
    
    dfs(root, target_sum, [])
    return result
```

### 8. Lowest Common Ancestor

```python
def lowest_common_ancestor(root: TreeNode, p: TreeNode, q: TreeNode) -> TreeNode:
    """
    Time: O(n)
    Space: O(h)
    """
    if not root or root == p or root == q:
        return root
    
    left = lowest_common_ancestor(root.left, p, q)
    right = lowest_common_ancestor(root.right, p, q)
    
    # Both found in different subtrees
    if left and right:
        return root
    
    # Return non-null result
    return left if left else right
```

### 9. Binary Tree Right Side View

```python
from collections import deque

def right_side_view(root: TreeNode) -> list[int]:
    """
    Return rightmost node at each level.
    
    Time: O(n)
    Space: O(w)
    """
    if not root:
        return []
    
    result = []
    queue = deque([root])
    
    while queue:
        level_size = len(queue)
        
        for i in range(level_size):
            node = queue.popleft()
            
            # Rightmost node of level
            if i == level_size - 1:
                result.append(node.val)
            
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)
    
    return result
```

### 10. Construct Tree from Traversals

```python
def build_tree(preorder: list[int], inorder: list[int]) -> TreeNode:
    """
    Build tree from preorder and inorder traversals.
    
    Time: O(n)
    Space: O(n)
    """
    if not preorder or not inorder:
        return None
    
    # Root is first in preorder
    root = TreeNode(preorder[0])
    
    # Find root in inorder
    mid = inorder.index(root.val)
    
    # Build subtrees
    root.left = build_tree(preorder[1:mid+1], inorder[:mid])
    root.right = build_tree(preorder[mid+1:], inorder[mid+1:])
    
    return root

# Optimized version
def build_tree_optimized(preorder: list[int], inorder: list[int]) -> TreeNode:
    """
    Time: O(n)
    Space: O(n)
    """
    inorder_map = {val: i for i, val in enumerate(inorder)}
    pre_idx = 0
    
    def build(left, right):
        nonlocal pre_idx
        
        if left > right:
            return None
        
        root_val = preorder[pre_idx]
        root = TreeNode(root_val)
        pre_idx += 1
        
        # Build left and right subtrees
        mid = inorder_map[root_val]
        root.left = build(left, mid - 1)
        root.right = build(mid + 1, right)
        
        return root
    
    return build(0, len(inorder) - 1)
```

### 11. Serialize and Deserialize

```python
class Codec:
    """Serialize and deserialize binary tree."""
    
    def serialize(self, root: TreeNode) -> str:
        """
        Encode tree to string.
        
        Time: O(n)
        Space: O(n)
        """
        result = []
        
        def dfs(node):
            if not node:
                result.append("null")
                return
            result.append(str(node.val))
            dfs(node.left)
            dfs(node.right)
        
        dfs(root)
        return ",".join(result)
    
    def deserialize(self, data: str) -> TreeNode:
        """
        Decode string to tree.
        
        Time: O(n)
        Space: O(n)
        """
        values = iter(data.split(","))
        
        def dfs():
            val = next(values)
            if val == "null":
                return None
            
            node = TreeNode(int(val))
            node.left = dfs()
            node.right = dfs()
            return node
        
        return dfs()
```

### 12. Maximum Path Sum

```python
def max_path_sum(root: TreeNode) -> int:
    """
    Find maximum path sum (any node to any node).
    
    Time: O(n)
    Space: O(h)
    """
    max_sum = float('-inf')
    
    def max_gain(node):
        nonlocal max_sum
        
        if not node:
            return 0
        
        # Max gain from left and right (ignore negative)
        left_gain = max(max_gain(node.left), 0)
        right_gain = max(max_gain(node.right), 0)
        
        # Path through current node
        current_path = node.val + left_gain + right_gain
        max_sum = max(max_sum, current_path)
        
        # Return max gain if continue from current node
        return node.val + max(left_gain, right_gain)
    
    max_gain(root)
    return max_sum
```

---

## Interview Tips

### 1. Recognize Tree Problems

```python
# Tree problems often involve:
- Recursion (natural fit)
- DFS vs BFS choice
- Path finding
- Level processing
- Tree construction
- Tree modification
```

### 2. DFS vs BFS

```python
# Use DFS (recursion/stack) for:
- Depth-related problems
- Path problems
- Tree construction
- Simple traversals

# Use BFS (queue) for:
- Level-order problems
- Shortest path
- Width-related problems
- Right/left side view
```

### 3. Recursive Template

```python
def solve_tree(root):
    # Base case
    if not root:
        return base_value
    
    # Process current node
    current_result = process(root.val)
    
    # Recursive calls
    left_result = solve_tree(root.left)
    right_result = solve_tree(root.right)
    
    # Combine results
    return combine(current_result, left_result, right_result)
```

### 4. Common Mistakes

```python
# ❌ Forgetting base case
def height(root):
    return 1 + max(height(root.left), height(root.right))  # Error!

# ✅ Always handle None
def height(root):
    if not root:
        return 0
    return 1 + max(height(root.left), height(root.right))

# ❌ Not considering leaf nodes
# ✅ Check: not root.left and not root.right
```

---

## Practice Problems

### Easy
1. [Maximum Depth](https://leetcode.com/problems/maximum-depth-of-binary-tree/)
2. [Same Tree](https://leetcode.com/problems/same-tree/)
3. [Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/)
4. [Symmetric Tree](https://leetcode.com/problems/symmetric-tree/)
5. [Path Sum](https://leetcode.com/problems/path-sum/)
6. [Merge Two Binary Trees](https://leetcode.com/problems/merge-two-binary-trees/)

### Medium
1. [Balanced Binary Tree](https://leetcode.com/problems/balanced-binary-tree/)
2. [Diameter of Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree/)
3. [Lowest Common Ancestor](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/)
4. [Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/)
5. [Zigzag Level Order](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/)
6. [Right Side View](https://leetcode.com/problems/binary-tree-right-side-view/)
7. [Construct from Preorder and Inorder](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)
8. [Path Sum II](https://leetcode.com/problems/path-sum-ii/)

### Hard
1. [Maximum Path Sum](https://leetcode.com/problems/binary-tree-maximum-path-sum/)
2. [Serialize and Deserialize](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/)
3. [Vertical Order Traversal](https://leetcode.com/problems/vertical-order-traversal-of-a-binary-tree/)

---

## Summary

### Key Takeaways
- ✅ Binary tree = Each node has ≤ 2 children
- ✅ Recursion is natural for trees
- ✅ DFS: Inorder, Preorder, Postorder
- ✅ BFS: Level order traversal
- ✅ Many problems use helper functions

### Common Patterns
```python
# 1. DFS recursive
def dfs(root):
    if not root:
        return base_case
    left = dfs(root.left)
    right = dfs(root.right)
    return combine(root.val, left, right)

# 2. BFS level order
from collections import deque
queue = deque([root])
while queue:
    level_size = len(queue)
    for _ in range(level_size):
        node = queue.popleft()
        # process node
        # add children

# 3. Path finding
def find_path(root, target, path):
    if not root:
        return False
    path.append(root.val)
    if root.val == target:
        return True
    if (find_path(root.left, target, path) or
        find_path(root.right, target, path)):
        return True
    path.pop()
    return False
```

---

**Next**: [Binary Search Trees →](../08-binary-search-trees/README.md)

**Happy Coding! 🚀**
