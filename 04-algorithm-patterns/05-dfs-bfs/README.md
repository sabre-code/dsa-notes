# 🌳 DFS & BFS - Python DSA

> Master depth-first and breadth-first traversals

---

## 📚 Table of Contents

1. [Introduction](#introduction)
2. [Depth-First Search (DFS)](#depth-first-search-dfs)
3. [Breadth-First Search (BFS)](#breadth-first-search-bfs)
4. [Tree Problems](#tree-problems)
5. [Graph Problems](#graph-problems)
6. [Matrix Problems](#matrix-problems)
7. [Interview Tips](#interview-tips)
8. [Practice Problems](#practice-problems)

---

## Introduction

**DFS and BFS** are fundamental traversal algorithms for trees and graphs.

### Key Characteristics
- ✅ **DFS** - Go deep first (stack/recursion)
- ✅ **BFS** - Go wide first (queue)
- ✅ **Different use cases** - Choose based on problem
- ✅ **O(V + E) time** - Visit each vertex and edge once

### When to Use

```python
# Use DFS for:
- Detecting cycles
- Topological sort
- Path finding (all paths)
- Connected components
- Tree problems (most)

# Use BFS for:
- Shortest path (unweighted)
- Level-order traversal
- Minimum steps/distance
- Closest/nearest problems
```

---

## Depth-First Search (DFS)

### DFS Templates

#### Recursive DFS (Tree)

```python
def dfs_recursive(root):
    """
    DFS recursive template for trees.
    
    Time: O(n)
    Space: O(h) where h is height
    """
    if not root:
        return
    
    # Process current node (preorder)
    print(root.val)
    
    # Recurse on children
    dfs_recursive(root.left)
    dfs_recursive(root.right)
```

#### Iterative DFS (Tree)

```python
def dfs_iterative(root):
    """
    DFS iterative template using stack.
    
    Time: O(n)
    Space: O(h)
    """
    if not root:
        return
    
    stack = [root]
    
    while stack:
        node = stack.pop()
        print(node.val)
        
        # Add right first (so left is processed first)
        if node.right:
            stack.append(node.right)
        if node.left:
            stack.append(node.left)
```

#### DFS for Graphs

```python
def dfs_graph(graph, start):
    """
    DFS for graphs with cycle prevention.
    
    Time: O(V + E)
    Space: O(V)
    """
    visited = set()
    
    def dfs(node):
        visited.add(node)
        print(node)
        
        for neighbor in graph.get(node, []):
            if neighbor not in visited:
                dfs(neighbor)
    
    dfs(start)
    return visited
```

---

## Breadth-First Search (BFS)

### BFS Templates

#### BFS for Trees

```python
from collections import deque

def bfs_tree(root):
    """
    BFS template for trees (level-order).
    
    Time: O(n)
    Space: O(w) where w is max width
    """
    if not root:
        return
    
    queue = deque([root])
    
    while queue:
        node = queue.popleft()
        print(node.val)
        
        if node.left:
            queue.append(node.left)
        if node.right:
            queue.append(node.right)
```

#### BFS with Levels

```python
def bfs_levels(root):
    """
    BFS tracking levels.
    
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
            level.append(node.val)
            
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)
        
        result.append(level)
    
    return result
```

#### BFS for Graphs

```python
def bfs_graph(graph, start):
    """
    BFS for graphs.
    
    Time: O(V + E)
    Space: O(V)
    """
    visited = {start}
    queue = deque([start])
    
    while queue:
        node = queue.popleft()
        print(node)
        
        for neighbor in graph.get(node, []):
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append(neighbor)
    
    return visited
```

---

## Tree Problems

### Problem 1: Maximum Depth of Binary Tree

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def max_depth_dfs(root: TreeNode) -> int:
    """
    Maximum depth using DFS.
    
    Time: O(n)
    Space: O(h)
    """
    if not root:
        return 0
    
    return 1 + max(max_depth_dfs(root.left), max_depth_dfs(root.right))

def max_depth_bfs(root: TreeNode) -> int:
    """
    Maximum depth using BFS.
    
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
        for _ in range(len(queue)):
            node = queue.popleft()
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)
    
    return depth
```

### Problem 2: Binary Tree Level Order Traversal

```python
def level_order(root: TreeNode) -> list[list[int]]:
    """
    Level-order traversal (BFS).
    
    Time: O(n)
    Space: O(w)
    
    Example: root = [3,9,20,null,null,15,7]
    Output: [[3],[9,20],[15,7]]
    """
    if not root:
        return []
    
    from collections import deque
    result = []
    queue = deque([root])
    
    while queue:
        level = []
        for _ in range(len(queue)):
            node = queue.popleft()
            level.append(node.val)
            
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)
        
        result.append(level)
    
    return result
```

### Problem 3: Binary Tree Zigzag Level Order

```python
def zigzag_level_order(root: TreeNode) -> list[list[int]]:
    """
    Zigzag level-order traversal.
    
    Time: O(n)
    Space: O(w)
    
    Example: root = [3,9,20,null,null,15,7]
    Output: [[3],[20,9],[15,7]]
    """
    if not root:
        return []
    
    from collections import deque
    result = []
    queue = deque([root])
    left_to_right = True
    
    while queue:
        level = []
        for _ in range(len(queue)):
            node = queue.popleft()
            level.append(node.val)
            
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)
        
        if not left_to_right:
            level.reverse()
        
        result.append(level)
        left_to_right = not left_to_right
    
    return result
```

### Problem 4: Binary Tree Right Side View

```python
def right_side_view(root: TreeNode) -> list[int]:
    """
    View tree from right side (BFS).
    
    Time: O(n)
    Space: O(w)
    
    Example: root = [1,2,3,null,5,null,4]
    Output: [1,3,4]
    """
    if not root:
        return []
    
    from collections import deque
    result = []
    queue = deque([root])
    
    while queue:
        level_size = len(queue)
        
        for i in range(level_size):
            node = queue.popleft()
            
            # Last node in level
            if i == level_size - 1:
                result.append(node.val)
            
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)
    
    return result
```

### Problem 5: Path Sum

```python
def has_path_sum(root: TreeNode, target_sum: int) -> bool:
    """
    Check if root-to-leaf path with target sum exists (DFS).
    
    Time: O(n)
    Space: O(h)
    
    Example: root = [5,4,8,11,null,13,4,7,2,null,null,null,1], target = 22
    Output: True
    """
    if not root:
        return False
    
    # Leaf node
    if not root.left and not root.right:
        return root.val == target_sum
    
    remaining = target_sum - root.val
    return (has_path_sum(root.left, remaining) or
            has_path_sum(root.right, remaining))
```

### Problem 6: All Paths From Root to Leaves

```python
def binary_tree_paths(root: TreeNode) -> list[str]:
    """
    Find all root-to-leaf paths (DFS).
    
    Time: O(n)
    Space: O(h)
    
    Example: root = [1,2,3,null,5]
    Output: ["1->2->5","1->3"]
    """
    if not root:
        return []
    
    paths = []
    
    def dfs(node, path):
        if not node.left and not node.right:
            paths.append(path + str(node.val))
            return
        
        if node.left:
            dfs(node.left, path + str(node.val) + "->")
        if node.right:
            dfs(node.right, path + str(node.val) + "->")
    
    dfs(root, "")
    return paths
```

### Problem 7: Lowest Common Ancestor

```python
def lowest_common_ancestor(root: TreeNode, p: TreeNode, q: TreeNode) -> TreeNode:
    """
    Find LCA of two nodes (DFS).
    
    Time: O(n)
    Space: O(h)
    
    Example: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
    Output: 3
    """
    if not root or root == p or root == q:
        return root
    
    left = lowest_common_ancestor(root.left, p, q)
    right = lowest_common_ancestor(root.right, p, q)
    
    if left and right:
        return root
    
    return left if left else right
```

### Problem 8: Serialize and Deserialize Binary Tree

```python
class Codec:
    """
    Serialize/deserialize binary tree.
    
    Time: O(n)
    Space: O(n)
    """
    def serialize(self, root: TreeNode) -> str:
        """Encode tree to string using BFS."""
        if not root:
            return ""
        
        from collections import deque
        result = []
        queue = deque([root])
        
        while queue:
            node = queue.popleft()
            if node:
                result.append(str(node.val))
                queue.append(node.left)
                queue.append(node.right)
            else:
                result.append("null")
        
        return ",".join(result)
    
    def deserialize(self, data: str) -> TreeNode:
        """Decode string to tree."""
        if not data:
            return None
        
        from collections import deque
        values = data.split(",")
        root = TreeNode(int(values[0]))
        queue = deque([root])
        i = 1
        
        while queue:
            node = queue.popleft()
            
            if i < len(values) and values[i] != "null":
                node.left = TreeNode(int(values[i]))
                queue.append(node.left)
            i += 1
            
            if i < len(values) and values[i] != "null":
                node.right = TreeNode(int(values[i]))
                queue.append(node.right)
            i += 1
        
        return root
```

---

## Graph Problems

### Problem 9: Clone Graph

```python
class Node:
    def __init__(self, val=0, neighbors=None):
        self.val = val
        self.neighbors = neighbors if neighbors else []

def clone_graph_dfs(node: Node) -> Node:
    """
    Deep copy graph using DFS.
    
    Time: O(V + E)
    Space: O(V)
    """
    if not node:
        return None
    
    clones = {}
    
    def dfs(node):
        if node in clones:
            return clones[node]
        
        clone = Node(node.val)
        clones[node] = clone
        
        for neighbor in node.neighbors:
            clone.neighbors.append(dfs(neighbor))
        
        return clone
    
    return dfs(node)

def clone_graph_bfs(node: Node) -> Node:
    """
    Deep copy graph using BFS.
    
    Time: O(V + E)
    Space: O(V)
    """
    if not node:
        return None
    
    from collections import deque
    clones = {node: Node(node.val)}
    queue = deque([node])
    
    while queue:
        curr = queue.popleft()
        
        for neighbor in curr.neighbors:
            if neighbor not in clones:
                clones[neighbor] = Node(neighbor.val)
                queue.append(neighbor)
            
            clones[curr].neighbors.append(clones[neighbor])
    
    return clones[node]
```

### Problem 10: Course Schedule (Detect Cycle)

```python
def can_finish(num_courses: int, prerequisites: list[list[int]]) -> bool:
    """
    Check if courses can be finished (detect cycle using DFS).
    
    Time: O(V + E)
    Space: O(V + E)
    
    Example: numCourses = 2, prerequisites = [[1,0]]
    Output: True
    """
    # Build graph
    from collections import defaultdict
    graph = defaultdict(list)
    for course, prereq in prerequisites:
        graph[course].append(prereq)
    
    # DFS with cycle detection
    WHITE, GRAY, BLACK = 0, 1, 2
    color = [WHITE] * num_courses
    
    def has_cycle(node):
        if color[node] == GRAY:
            return True
        if color[node] == BLACK:
            return False
        
        color[node] = GRAY
        
        for neighbor in graph[node]:
            if has_cycle(neighbor):
                return True
        
        color[node] = BLACK
        return False
    
    for course in range(num_courses):
        if color[course] == WHITE:
            if has_cycle(course):
                return False
    
    return True
```

### Problem 11: All Paths from Source to Target

```python
def all_paths_source_target(graph: list[list[int]]) -> list[list[int]]:
    """
    Find all paths from 0 to n-1 (DFS).
    
    Time: O(2^n * n)
    Space: O(n)
    
    Example: graph = [[1,2],[3],[3],[]]
    Output: [[0,1,3],[0,2,3]]
    """
    target = len(graph) - 1
    result = []
    
    def dfs(node, path):
        if node == target:
            result.append(path[:])
            return
        
        for neighbor in graph[node]:
            path.append(neighbor)
            dfs(neighbor, path)
            path.pop()
    
    dfs(0, [0])
    return result
```

---

## Matrix Problems

### Problem 12: Number of Islands

```python
def num_islands(grid: list[list[str]]) -> int:
    """
    Count islands using DFS.
    
    Time: O(m * n)
    Space: O(m * n)
    
    Example: grid = [
      ["1","1","0","0","0"],
      ["1","1","0","0","0"],
      ["0","0","1","0","0"],
      ["0","0","0","1","1"]
    ]
    Output: 3
    """
    if not grid:
        return 0
    
    rows, cols = len(grid), len(grid[0])
    count = 0
    
    def dfs(r, c):
        if (r < 0 or r >= rows or c < 0 or c >= cols or
            grid[r][c] != '1'):
            return
        
        grid[r][c] = '0'  # Mark visited
        
        dfs(r + 1, c)
        dfs(r - 1, c)
        dfs(r, c + 1)
        dfs(r, c - 1)
    
    for r in range(rows):
        for c in range(cols):
            if grid[r][c] == '1':
                dfs(r, c)
                count += 1
    
    return count
```

### Problem 13: Max Area of Island

```python
def max_area_of_island(grid: list[list[int]]) -> int:
    """
    Find maximum island area (DFS).
    
    Time: O(m * n)
    Space: O(m * n)
    
    Example: grid = [[0,0,1,0,0],[0,1,1,1,0],[0,1,0,0,0]]
    Output: 4
    """
    if not grid:
        return 0
    
    rows, cols = len(grid), len(grid[0])
    max_area = 0
    
    def dfs(r, c):
        if (r < 0 or r >= rows or c < 0 or c >= cols or
            grid[r][c] != 1):
            return 0
        
        grid[r][c] = 0
        
        return (1 + dfs(r + 1, c) + dfs(r - 1, c) +
                dfs(r, c + 1) + dfs(r, c - 1))
    
    for r in range(rows):
        for c in range(cols):
            if grid[r][c] == 1:
                max_area = max(max_area, dfs(r, c))
    
    return max_area
```

### Problem 14: Surrounded Regions

```python
def solve(board: list[list[str]]) -> None:
    """
    Capture surrounded regions (DFS from borders).
    
    Time: O(m * n)
    Space: O(m * n)
    
    Modifies board in-place.
    """
    if not board:
        return
    
    rows, cols = len(board), len(board[0])
    
    def dfs(r, c):
        if (r < 0 or r >= rows or c < 0 or c >= cols or
            board[r][c] != 'O'):
            return
        
        board[r][c] = 'T'  # Temporary mark
        
        dfs(r + 1, c)
        dfs(r - 1, c)
        dfs(r, c + 1)
        dfs(r, c - 1)
    
    # Mark border-connected 'O's
    for r in range(rows):
        dfs(r, 0)
        dfs(r, cols - 1)
    
    for c in range(cols):
        dfs(0, c)
        dfs(rows - 1, c)
    
    # Flip remaining 'O's to 'X' and 'T's back to 'O'
    for r in range(rows):
        for c in range(cols):
            if board[r][c] == 'O':
                board[r][c] = 'X'
            elif board[r][c] == 'T':
                board[r][c] = 'O'
```

### Problem 15: Shortest Path in Binary Matrix

```python
def shortest_path_binary_matrix(grid: list[list[int]]) -> int:
    """
    Shortest path from top-left to bottom-right (BFS).
    
    Time: O(n²)
    Space: O(n²)
    
    Example: grid = [[0,0,0],[1,1,0],[1,1,0]]
    Output: 4
    """
    n = len(grid)
    if grid[0][0] == 1 or grid[n-1][n-1] == 1:
        return -1
    
    from collections import deque
    queue = deque([(0, 0, 1)])  # (row, col, distance)
    grid[0][0] = 1  # Mark visited
    
    directions = [(-1,-1),(-1,0),(-1,1),(0,-1),(0,1),(1,-1),(1,0),(1,1)]
    
    while queue:
        r, c, dist = queue.popleft()
        
        if r == n - 1 and c == n - 1:
            return dist
        
        for dr, dc in directions:
            nr, nc = r + dr, c + dc
            
            if (0 <= nr < n and 0 <= nc < n and grid[nr][nc] == 0):
                grid[nr][nc] = 1
                queue.append((nr, nc, dist + 1))
    
    return -1
```

### Problem 16: Rotting Oranges

```python
def oranges_rotting(grid: list[list[int]]) -> int:
    """
    Minimum time for all oranges to rot (Multi-source BFS).
    
    Time: O(m * n)
    Space: O(m * n)
    
    Example: grid = [[2,1,1],[1,1,0],[0,1,1]]
    Output: 4
    """
    from collections import deque
    
    rows, cols = len(grid), len(grid[0])
    queue = deque()
    fresh = 0
    
    # Find all rotten oranges and count fresh
    for r in range(rows):
        for c in range(cols):
            if grid[r][c] == 2:
                queue.append((r, c, 0))
            elif grid[r][c] == 1:
                fresh += 1
    
    if fresh == 0:
        return 0
    
    minutes = 0
    directions = [(0,1), (1,0), (0,-1), (-1,0)]
    
    while queue:
        r, c, time = queue.popleft()
        minutes = max(minutes, time)
        
        for dr, dc in directions:
            nr, nc = r + dr, c + dc
            
            if (0 <= nr < rows and 0 <= nc < cols and
                grid[nr][nc] == 1):
                grid[nr][nc] = 2
                fresh -= 1
                queue.append((nr, nc, time + 1))
    
    return minutes if fresh == 0 else -1
```

---

## Interview Tips

### 1. DFS vs BFS Choice

```python
# Use DFS when:
✅ Exploring all paths
✅ Detecting cycles
✅ Topological sort
✅ Memory is concern (recursion)
✅ Tree problems (usually)

# Use BFS when:
✅ Shortest path (unweighted)
✅ Level-order traversal
✅ Minimum distance/steps
✅ Closest node problems
```

### 2. Common Patterns

```python
# DFS Pattern (Recursive)
def dfs(node):
    if not node:
        return
    # Process node
    dfs(node.left)
    dfs(node.right)

# DFS Pattern (Iterative)
stack = [start]
visited = set()
while stack:
    node = stack.pop()
    if node not in visited:
        visited.add(node)
        for neighbor in graph[node]:
            stack.append(neighbor)

# BFS Pattern
from collections import deque
queue = deque([start])
visited = {start}
while queue:
    node = queue.popleft()
    for neighbor in graph[node]:
        if neighbor not in visited:
            visited.add(neighbor)
            queue.append(neighbor)

# Matrix DFS/BFS (4 directions)
directions = [(0,1), (1,0), (0,-1), (-1,0)]
for dr, dc in directions:
    nr, nc = r + dr, c + dc
    if 0 <= nr < rows and 0 <= nc < cols:
        # Process neighbor
```

### 3. Time & Space Complexity

```python
# Trees:
Time: O(n) - visit each node once
Space: O(h) DFS recursion, O(w) BFS queue

# Graphs:
Time: O(V + E) - visit vertices and edges
Space: O(V) - visited set

# Matrix:
Time: O(m * n) - visit each cell
Space: O(m * n) - recursion/queue
```

### 4. Common Mistakes

```python
# ❌ Forgetting to mark visited
visited.add(node)  # Add this!

# ❌ Wrong queue import
from collections import deque  # Not list!

# ❌ Not handling empty input
if not root:
    return

# ❌ Modifying while iterating
# Use list() or iterate over copy
```

---

## Practice Problems

### Easy
1. [Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/)
2. [Same Tree](https://leetcode.com/problems/same-tree/)
3. [Symmetric Tree](https://leetcode.com/problems/symmetric-tree/)
4. [Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/)
5. [Path Sum](https://leetcode.com/problems/path-sum/)

### Medium
1. [Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/)
2. [Binary Tree Zigzag Level Order](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/)
3. [Binary Tree Right Side View](https://leetcode.com/problems/binary-tree-right-side-view/)
4. [All Paths From Source to Target](https://leetcode.com/problems/all-paths-from-source-to-target/)
5. [Clone Graph](https://leetcode.com/problems/clone-graph/)
6. [Course Schedule](https://leetcode.com/problems/course-schedule/)
7. [Number of Islands](https://leetcode.com/problems/number-of-islands/)
8. [Max Area of Island](https://leetcode.com/problems/max-area-of-island/)
9. [Surrounded Regions](https://leetcode.com/problems/surrounded-regions/)
10. [Rotting Oranges](https://leetcode.com/problems/rotting-oranges/)

### Hard
1. [Serialize and Deserialize Binary Tree](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/)
2. [Word Ladder](https://leetcode.com/problems/word-ladder/)
3. [Shortest Path in Binary Matrix](https://leetcode.com/problems/shortest-path-in-a-binary-matrix/)

---

## Summary

### Key Takeaways
- ✅ **DFS** - Stack/recursion, go deep
- ✅ **BFS** - Queue, go wide, shortest path
- ✅ **Both O(V + E)** - Efficient traversal
- ✅ **Choose wisely** - Based on problem requirements

### Quick Reference

```python
# DFS Recursive
def dfs(node):
    if not node:
        return
    # Process
    dfs(node.left)
    dfs(node.right)

# DFS Iterative
stack = [root]
while stack:
    node = stack.pop()
    # Process
    if node.right:
        stack.append(node.right)
    if node.left:
        stack.append(node.left)

# BFS
from collections import deque
queue = deque([root])
while queue:
    node = queue.popleft()
    # Process
    if node.left:
        queue.append(node.left)
    if node.right:
        queue.append(node.right)
```

---

**Next**: [Greedy Algorithms →](../06-greedy/README.md)

**Happy Coding! 🚀**
