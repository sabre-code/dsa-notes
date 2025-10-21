# 📝 Quick Reference Cheat Sheet

> Essential Python DSA syntax and patterns for interviews

---

## Python Basics

### Lists
```python
# Create
arr = [1, 2, 3]
arr = [0] * n                    # [0, 0, ..., 0]
arr = list(range(n))             # [0, 1, ..., n-1]
arr = [x for x in range(n)]      # List comprehension

# Access
first = arr[0]
last = arr[-1]
subset = arr[1:3]                # [start:end]
reverse = arr[::-1]

# Modify
arr.append(x)                    # O(1) amortized
arr.insert(i, x)                 # O(n)
arr.pop()                        # O(1), remove last
arr.pop(i)                       # O(n), remove at index
arr.remove(x)                    # O(n), remove first occurrence

# Operations
len(arr)
sum(arr)
min(arr), max(arr)
arr.sort()                       # O(n log n), in-place
sorted(arr)                      # O(n log n), returns new
arr.reverse()                    # O(n), in-place
arr.count(x)                     # O(n)
arr.index(x)                     # O(n)
x in arr                         # O(n)
```

### Strings
```python
s = "hello"
s[0], s[-1]                      # 'h', 'o'
s[1:4]                           # 'ell'
s[::-1]                          # 'olleh' (reverse)

s.lower(), s.upper()
s.strip()                        # Remove whitespace
s.split()                        # Split by whitespace
'-'.join(['a', 'b'])             # 'a-b'
s.replace('l', 'L')              # 'heLLo'
s.isalnum(), s.isalpha()
s.startswith('he'), s.endswith('lo')
```

### Dictionaries
```python
d = {}
d = {'a': 1, 'b': 2}
d = dict([('a', 1), ('b', 2)])

d['a']                           # 1, KeyError if not found
d.get('a', 0)                    # 0 if not found
d.setdefault('a', 0)             # Set if not exists

d.keys(), d.values(), d.items()
'a' in d                         # O(1)
del d['a']
d.pop('a', None)
```

### Sets
```python
s = set()
s = {1, 2, 3}

s.add(x)
s.remove(x)                      # KeyError if not found
s.discard(x)                     # No error if not found
x in s                           # O(1)

# Set operations
s1 | s2                          # Union
s1 & s2                          # Intersection
s1 - s2                          # Difference
s1 ^ s2                          # Symmetric difference
```

---

## Collections Module

```python
from collections import Counter, defaultdict, deque

# Counter - frequency counting
count = Counter([1, 1, 2, 3, 3, 3])
count.most_common(2)             # [(3, 3), (1, 2)]

# defaultdict - auto-initialize
graph = defaultdict(list)
graph['a'].append('b')           # No KeyError

# deque - efficient queue
q = deque([1, 2, 3])
q.append(4)                      # Add right
q.appendleft(0)                  # Add left
q.pop()                          # Remove right
q.popleft()                      # Remove left
```

---

## heapq (Min Heap)

```python
import heapq

heap = []
heapq.heappush(heap, 3)          # Add element
smallest = heapq.heappop(heap)   # Remove smallest

heapq.heapify(arr)               # Convert to heap in-place

# k smallest/largest
heapq.nsmallest(k, arr)
heapq.nlargest(k, arr)

# Max heap (negate values)
max_heap = [-x for x in arr]
heapq.heapify(max_heap)
```

---

## Patterns Cheat Sheet

### Two Pointers
```python
# Opposite direction
left, right = 0, len(arr) - 1
while left < right:
    if condition:
        left += 1
    else:
        right -= 1

# Same direction (fast & slow)
slow = 0
for fast in range(len(arr)):
    if condition:
        arr[slow] = arr[fast]
        slow += 1
```

### Sliding Window
```python
left = 0
for right in range(len(arr)):
    # Add arr[right] to window
    
    while window_invalid:
        # Remove arr[left] from window
        left += 1
    
    # Update result
```

### Binary Search
```python
left, right = 0, len(arr) - 1

while left <= right:
    mid = (left + right) // 2
    
    if arr[mid] == target:
        return mid
    elif arr[mid] < target:
        left = mid + 1
    else:
        right = mid - 1
```

### DFS (Recursion)
```python
def dfs(node, visited):
    if node in visited:
        return
    
    visited.add(node)
    
    for neighbor in graph[node]:
        dfs(neighbor, visited)
```

### BFS (Queue)
```python
from collections import deque

queue = deque([start])
visited = {start}

while queue:
    node = queue.popleft()
    
    for neighbor in graph[node]:
        if neighbor not in visited:
            visited.add(neighbor)
            queue.append(neighbor)
```

### Backtracking
```python
def backtrack(path, choices):
    if is_solution(path):
        result.append(path[:])
        return
    
    for choice in choices:
        # Make choice
        path.append(choice)
        
        # Recurse
        backtrack(path, remaining_choices)
        
        # Undo choice
        path.pop()
```

### Dynamic Programming
```python
# 1D DP
dp = [0] * (n + 1)
for i in range(1, n + 1):
    dp[i] = some_function(dp[i-1], dp[i-2])

# 2D DP
dp = [[0] * (n + 1) for _ in range(m + 1)]
for i in range(1, m + 1):
    for j in range(1, n + 1):
        dp[i][j] = some_function(dp[i-1][j], dp[i][j-1])
```

---

## Common Algorithms

### Sort & Search
```python
# Sort
arr.sort()                       # O(n log n), in-place
sorted_arr = sorted(arr)         # O(n log n), new list

# Binary search (sorted array)
import bisect
index = bisect.bisect_left(arr, x)

# Linear search
if x in arr:                     # O(n)
    index = arr.index(x)
```

### Reverse
```python
# String
reversed_str = s[::-1]
reversed_str = ''.join(reversed(s))

# List
arr.reverse()                    # In-place
reversed_arr = arr[::-1]         # New list
```

### Enumerate & Zip
```python
# Enumerate (index + value)
for i, val in enumerate(arr):
    print(i, val)

# Zip (parallel iteration)
for a, b in zip(arr1, arr2):
    print(a, b)
```

---

## Tree Traversal

```python
class TreeNode:
    def __init__(self, val=0):
        self.val = val
        self.left = None
        self.right = None

# Inorder (Left, Root, Right)
def inorder(root):
    if not root:
        return []
    return inorder(root.left) + [root.val] + inorder(root.right)

# Preorder (Root, Left, Right)
def preorder(root):
    if not root:
        return []
    return [root.val] + preorder(root.left) + preorder(root.right)

# Postorder (Left, Right, Root)
def postorder(root):
    if not root:
        return []
    return postorder(root.left) + postorder(root.right) + [root.val]

# Level Order (BFS)
from collections import deque

def levelorder(root):
    if not root:
        return []
    
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

---

## Complexity Reference

### Time Complexity
```
O(1)         - Constant
O(log n)     - Logarithmic (binary search, heap ops)
O(n)         - Linear (single loop)
O(n log n)   - Linearithmic (sorting)
O(n²)        - Quadratic (nested loops)
O(2ⁿ)        - Exponential (subsets)
O(n!)        - Factorial (permutations)
```

### Space Complexity
```
O(1)    - Constant (few variables)
O(n)    - Linear (array, hash map, recursion depth)
O(n²)   - Quadratic (2D array)
```

---

## Common Tricks

### Infinity
```python
float('inf')                     # Positive infinity
float('-inf')                    # Negative infinity
```

### Swapping
```python
a, b = b, a                      # Pythonic swap
```

### Multiple Assignment
```python
x, y, z = 1, 2, 3
first, *middle, last = arr       # Unpacking
```

### Checking Empty
```python
if not arr:                      # Better than len(arr) == 0
    ...
```

### Range with Step
```python
range(n)                         # 0 to n-1
range(a, b)                      # a to b-1
range(a, b, step)                # a to b-1, increment by step
```

### All/Any
```python
all([True, True, False])         # False
any([True, False, False])        # True
```

### Math Operations
```python
abs(-5)                          # 5
pow(2, 3)                        # 8
divmod(10, 3)                    # (3, 1) quotient, remainder
min(a, b, c)
max(a, b, c)
```

---

## Interview Template

```python
def solve_problem(input):
    """
    Problem: [Brief description]
    
    Approach: [High-level strategy]
    
    Time Complexity: O(?)
    Space Complexity: O(?)
    
    Args:
        input: [Description]
    
    Returns:
        [Description]
    """
    # Edge cases
    if not input:
        return []
    
    # Main logic
    result = []
    
    # ... your code ...
    
    return result

# Test cases
assert solve_problem([1, 2, 3]) == expected_output
```

---

## Problem-Solving Checklist

### Before Coding
- [ ] Understand the problem
- [ ] Ask clarifying questions
- [ ] Think of edge cases
- [ ] Choose approach & pattern
- [ ] Estimate time/space complexity

### While Coding
- [ ] Use meaningful variable names
- [ ] Write clean, readable code
- [ ] Handle edge cases
- [ ] Add comments for complex logic

### After Coding
- [ ] Test with examples
- [ ] Walk through the code
- [ ] Check edge cases
- [ ] Analyze complexity
- [ ] Discuss optimizations

---

## Must-Know for Interviews

### Data Structures
✅ Arrays/Lists  
✅ Strings  
✅ Hash Tables  
✅ Linked Lists  
✅ Stacks/Queues  
✅ Trees (Binary, BST)  
✅ Graphs  
✅ Heaps  

### Algorithms
✅ Two Pointers  
✅ Sliding Window  
✅ Binary Search  
✅ DFS/BFS  
✅ Backtracking  
✅ Dynamic Programming  
✅ Greedy  
✅ Sorting  

### Patterns
✅ Hash Map for O(1) lookup  
✅ Two Pointers for pairs  
✅ Sliding Window for subarrays  
✅ DFS for trees/graphs  
✅ BFS for shortest path  
✅ DP for optimization  
✅ Backtracking for combinations  

---

**Print this sheet and keep it handy during practice! 🚀**
