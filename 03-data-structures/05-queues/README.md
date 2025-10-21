# 🎫 Queues - Python DSA

> First In, First Out (FIFO) data structure

---

## 📚 Table of Contents

1. [Introduction](#introduction)
2. [Types of Queues](#types-of-queues)
3. [Implementation](#implementation)
4. [Common Patterns](#common-patterns)
5. [Classic Problems](#classic-problems)
6. [Interview Tips](#interview-tips)
7. [Practice Problems](#practice-problems)

---

## Introduction

**Queue** is a linear data structure that follows FIFO (First In, First Out) principle.

### Key Characteristics
- ✅ **FIFO access** - First element added is first removed
- ✅ **O(1) enqueue/dequeue** - Constant time operations
- ✅ **Front and rear pointers** - Access both ends
- ❌ **No random access** - Can only access front

### Real-World Examples
- 🎫 Ticket counter queue
- 🖨️ Printer queue
- 📞 Call center waiting
- 🚗 Traffic management
- 🎮 Game matchmaking

### Queue Operations

| Operation | Time | Description |
|-----------|------|-------------|
| enqueue(item) | O(1) | Add to rear |
| dequeue() | O(1) | Remove from front |
| front() | O(1) | View front element |
| rear() | O(1) | View rear element |
| is_empty() | O(1) | Check if empty |
| size() | O(1) | Get size |

---

## Types of Queues

### 1. Simple Queue
Standard FIFO queue.

### 2. Circular Queue
Last position connected back to first position.

### 3. Priority Queue
Elements have priorities; highest priority dequeued first.

### 4. Double-ended Queue (Deque)
Can add/remove from both ends.

---

## Implementation

### Using collections.deque

```python
from collections import deque

class Queue:
    """Queue using collections.deque (most efficient)."""
    
    def __init__(self):
        self.items = deque()
    
    def enqueue(self, item):
        """Add to rear. Time: O(1)"""
        self.items.append(item)
    
    def dequeue(self):
        """Remove from front. Time: O(1)"""
        if self.is_empty():
            raise IndexError("Queue is empty")
        return self.items.popleft()
    
    def front(self):
        """View front element. Time: O(1)"""
        if self.is_empty():
            raise IndexError("Queue is empty")
        return self.items[0]
    
    def rear(self):
        """View rear element. Time: O(1)"""
        if self.is_empty():
            raise IndexError("Queue is empty")
        return self.items[-1]
    
    def is_empty(self):
        """Check if empty. Time: O(1)"""
        return len(self.items) == 0
    
    def size(self):
        """Get size. Time: O(1)"""
        return len(self.items)
    
    def __repr__(self):
        return f"Queue({list(self.items)})"

# Test
queue = Queue()
queue.enqueue(1)
queue.enqueue(2)
queue.enqueue(3)
print(queue.dequeue())  # 1
print(queue.front())    # 2
```

### Using List (Less Efficient)

```python
class ListQueue:
    """Queue using Python list (O(n) dequeue)."""
    
    def __init__(self):
        self.items = []
    
    def enqueue(self, item):
        """Time: O(1)"""
        self.items.append(item)
    
    def dequeue(self):
        """Time: O(n) - requires shifting elements"""
        if self.is_empty():
            raise IndexError("Queue is empty")
        return self.items.pop(0)
    
    def is_empty(self):
        return len(self.items) == 0
```

### Circular Queue

```python
class CircularQueue:
    """Circular queue with fixed capacity."""
    
    def __init__(self, capacity):
        self.capacity = capacity
        self.items = [None] * capacity
        self.front = 0
        self.rear = -1
        self.size = 0
    
    def enqueue(self, item):
        """Time: O(1)"""
        if self.is_full():
            raise OverflowError("Queue is full")
        
        self.rear = (self.rear + 1) % self.capacity
        self.items[self.rear] = item
        self.size += 1
    
    def dequeue(self):
        """Time: O(1)"""
        if self.is_empty():
            raise IndexError("Queue is empty")
        
        item = self.items[self.front]
        self.items[self.front] = None
        self.front = (self.front + 1) % self.capacity
        self.size -= 1
        return item
    
    def is_empty(self):
        return self.size == 0
    
    def is_full(self):
        return self.size == self.capacity
    
    def __len__(self):
        return self.size

# Test
cq = CircularQueue(5)
for i in range(1, 6):
    cq.enqueue(i)
print(cq.dequeue())  # 1
print(cq.dequeue())  # 2
cq.enqueue(6)
cq.enqueue(7)
```

### Priority Queue using heapq

```python
import heapq

class PriorityQueue:
    """Min heap-based priority queue."""
    
    def __init__(self):
        self.heap = []
        self.counter = 0  # For tie-breaking
    
    def enqueue(self, item, priority):
        """Time: O(log n)"""
        # Use counter for FIFO when priorities are equal
        heapq.heappush(self.heap, (priority, self.counter, item))
        self.counter += 1
    
    def dequeue(self):
        """Time: O(log n)"""
        if self.is_empty():
            raise IndexError("Queue is empty")
        return heapq.heappop(self.heap)[2]
    
    def peek(self):
        """Time: O(1)"""
        if self.is_empty():
            raise IndexError("Queue is empty")
        return self.heap[0][2]
    
    def is_empty(self):
        return len(self.heap) == 0
    
    def size(self):
        return len(self.heap)

# Test
pq = PriorityQueue()
pq.enqueue("task1", 3)
pq.enqueue("task2", 1)
pq.enqueue("task3", 2)
print(pq.dequeue())  # task2 (priority 1)
print(pq.dequeue())  # task3 (priority 2)
```

### Double-ended Queue (Deque)

```python
from collections import deque

class Deque:
    """Wrapper around collections.deque."""
    
    def __init__(self):
        self.items = deque()
    
    def add_front(self, item):
        """Time: O(1)"""
        self.items.appendleft(item)
    
    def add_rear(self, item):
        """Time: O(1)"""
        self.items.append(item)
    
    def remove_front(self):
        """Time: O(1)"""
        if self.is_empty():
            raise IndexError("Deque is empty")
        return self.items.popleft()
    
    def remove_rear(self):
        """Time: O(1)"""
        if self.is_empty():
            raise IndexError("Deque is empty")
        return self.items.pop()
    
    def peek_front(self):
        """Time: O(1)"""
        if self.is_empty():
            raise IndexError("Deque is empty")
        return self.items[0]
    
    def peek_rear(self):
        """Time: O(1)"""
        if self.is_empty():
            raise IndexError("Deque is empty")
        return self.items[-1]
    
    def is_empty(self):
        return len(self.items) == 0
    
    def size(self):
        return len(self.items)
```

---

## Common Patterns

### Pattern 1: BFS (Breadth-First Search)

```python
from collections import deque

def bfs(graph, start):
    """
    BFS traversal using queue.
    
    Time: O(V + E)
    Space: O(V)
    """
    visited = set()
    queue = deque([start])
    visited.add(start)
    result = []
    
    while queue:
        node = queue.popleft()
        result.append(node)
        
        for neighbor in graph[node]:
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append(neighbor)
    
    return result

# Example:
graph = {
    'A': ['B', 'C'],
    'B': ['D', 'E'],
    'C': ['F'],
    'D': [],
    'E': ['F'],
    'F': []
}
print(bfs(graph, 'A'))  # ['A', 'B', 'C', 'D', 'E', 'F']
```

### Pattern 2: Level Order Traversal

```python
def level_order(root):
    """
    Level order traversal of binary tree.
    
    Time: O(n)
    Space: O(n)
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

### Pattern 3: Sliding Window Maximum

```python
from collections import deque

def max_sliding_window(nums: list[int], k: int) -> list[int]:
    """
    Find max in each sliding window using monotonic deque.
    
    Time: O(n)
    Space: O(k)
    """
    result = []
    dq = deque()  # Store indices
    
    for i in range(len(nums)):
        # Remove elements outside window
        while dq and dq[0] < i - k + 1:
            dq.popleft()
        
        # Remove smaller elements (maintain decreasing order)
        while dq and nums[dq[-1]] < nums[i]:
            dq.pop()
        
        dq.append(i)
        
        # Add to result starting from kth element
        if i >= k - 1:
            result.append(nums[dq[0]])
    
    return result

# Example:
# Input: nums = [1,3,-1,-3,5,3,6,7], k = 3
# Output: [3,3,5,5,6,7]
```

---

## Classic Problems

### 1. Implement Queue using Stacks

```python
class MyQueue:
    """
    Queue using two stacks.
    """
    
    def __init__(self):
        self.stack_in = []
        self.stack_out = []
    
    def push(self, x: int) -> None:
        """Time: O(1)"""
        self.stack_in.append(x)
    
    def pop(self) -> int:
        """Amortized Time: O(1)"""
        self._move_if_needed()
        return self.stack_out.pop()
    
    def peek(self) -> int:
        """Amortized Time: O(1)"""
        self._move_if_needed()
        return self.stack_out[-1]
    
    def empty(self) -> bool:
        """Time: O(1)"""
        return not self.stack_in and not self.stack_out
    
    def _move_if_needed(self):
        """Move elements from stack_in to stack_out if needed."""
        if not self.stack_out:
            while self.stack_in:
                self.stack_out.append(self.stack_in.pop())
```

### 2. Number of Recent Calls

```python
from collections import deque

class RecentCounter:
    """
    Count requests in last 3000ms.
    """
    
    def __init__(self):
        self.requests = deque()
    
    def ping(self, t: int) -> int:
        """
        Time: O(1) amortized
        """
        self.requests.append(t)
        
        # Remove old requests
        while self.requests[0] < t - 3000:
            self.requests.popleft()
        
        return len(self.requests)

# Test
counter = RecentCounter()
print(counter.ping(1))     # 1
print(counter.ping(100))   # 2
print(counter.ping(3001))  # 3
print(counter.ping(3002))  # 3
```

### 3. Design Circular Queue

```python
class MyCircularQueue:
    """
    Circular queue with fixed capacity.
    """
    
    def __init__(self, k: int):
        self.capacity = k
        self.queue = [0] * k
        self.head = 0
        self.size = 0
    
    def enQueue(self, value: int) -> bool:
        """Time: O(1)"""
        if self.isFull():
            return False
        
        tail = (self.head + self.size) % self.capacity
        self.queue[tail] = value
        self.size += 1
        return True
    
    def deQueue(self) -> bool:
        """Time: O(1)"""
        if self.isEmpty():
            return False
        
        self.head = (self.head + 1) % self.capacity
        self.size -= 1
        return True
    
    def Front(self) -> int:
        """Time: O(1)"""
        return -1 if self.isEmpty() else self.queue[self.head]
    
    def Rear(self) -> int:
        """Time: O(1)"""
        if self.isEmpty():
            return -1
        tail = (self.head + self.size - 1) % self.capacity
        return self.queue[tail]
    
    def isEmpty(self) -> bool:
        """Time: O(1)"""
        return self.size == 0
    
    def isFull(self) -> bool:
        """Time: O(1)"""
        return self.size == self.capacity
```

### 4. Moving Average from Data Stream

```python
from collections import deque

class MovingAverage:
    """
    Calculate moving average of last n values.
    """
    
    def __init__(self, size: int):
        self.size = size
        self.queue = deque()
        self.sum = 0
    
    def next(self, val: int) -> float:
        """Time: O(1)"""
        self.queue.append(val)
        self.sum += val
        
        # Remove oldest if exceeded size
        if len(self.queue) > self.size:
            self.sum -= self.queue.popleft()
        
        return self.sum / len(self.queue)

# Test
ma = MovingAverage(3)
print(ma.next(1))   # 1.0
print(ma.next(10))  # 5.5
print(ma.next(3))   # 4.666...
print(ma.next(5))   # 6.0
```

### 5. Rotting Oranges (BFS)

```python
from collections import deque

def oranges_rotting(grid: list[list[int]]) -> int:
    """
    Time: O(m * n)
    Space: O(m * n)
    """
    rows, cols = len(grid), len(grid[0])
    queue = deque()
    fresh = 0
    
    # Find all rotten oranges and count fresh
    for r in range(rows):
        for c in range(cols):
            if grid[r][c] == 2:
                queue.append((r, c, 0))  # (row, col, time)
            elif grid[r][c] == 1:
                fresh += 1
    
    minutes = 0
    directions = [(0, 1), (1, 0), (0, -1), (-1, 0)]
    
    # BFS
    while queue:
        r, c, minutes = queue.popleft()
        
        for dr, dc in directions:
            nr, nc = r + dr, c + dc
            
            # If fresh orange, rot it
            if 0 <= nr < rows and 0 <= nc < cols and grid[nr][nc] == 1:
                grid[nr][nc] = 2
                fresh -= 1
                queue.append((nr, nc, minutes + 1))
    
    return minutes if fresh == 0 else -1

# Example:
# Input: [[2,1,1],[1,1,0],[0,1,1]]
# Output: 4
```

### 6. Perfect Squares (BFS)

```python
from collections import deque

def num_squares(n: int) -> int:
    """
    Find minimum perfect squares that sum to n.
    
    Time: O(n * sqrt(n))
    Space: O(n)
    """
    # Generate perfect squares up to n
    squares = [i * i for i in range(1, int(n**0.5) + 1)]
    
    queue = deque([(n, 0)])  # (remaining, steps)
    visited = {n}
    
    while queue:
        remaining, steps = queue.popleft()
        
        for square in squares:
            next_remaining = remaining - square
            
            if next_remaining == 0:
                return steps + 1
            
            if next_remaining > 0 and next_remaining not in visited:
                visited.add(next_remaining)
                queue.append((next_remaining, steps + 1))
    
    return 0

# Example:
# Input: n = 12
# Output: 3  # 12 = 4 + 4 + 4
```

### 7. Open the Lock (BFS)

```python
from collections import deque

def open_lock(deadends: list[str], target: str) -> int:
    """
    Time: O(10000 * 4 * 2) = O(1)
    Space: O(10000)
    """
    dead = set(deadends)
    if "0000" in dead:
        return -1
    
    queue = deque([("0000", 0)])
    visited = {"0000"}
    
    def neighbors(code):
        """Get all possible next codes."""
        result = []
        for i in range(4):
            digit = int(code[i])
            for move in [-1, 1]:
                new_digit = (digit + move) % 10
                new_code = code[:i] + str(new_digit) + code[i+1:]
                result.append(new_code)
        return result
    
    while queue:
        code, steps = queue.popleft()
        
        if code == target:
            return steps
        
        for neighbor in neighbors(code):
            if neighbor not in visited and neighbor not in dead:
                visited.add(neighbor)
                queue.append((neighbor, steps + 1))
    
    return -1
```

### 8. Shortest Bridge (BFS + DFS)

```python
from collections import deque

def shortest_bridge(grid: list[list[int]]) -> int:
    """
    Time: O(n^2)
    Space: O(n^2)
    """
    n = len(grid)
    directions = [(0, 1), (1, 0), (0, -1), (-1, 0)]
    
    def dfs(r, c, queue):
        """Find all cells of first island."""
        if r < 0 or r >= n or c < 0 or c >= n or grid[r][c] != 1:
            return
        
        grid[r][c] = 2  # Mark as visited
        queue.append((r, c, 0))
        
        for dr, dc in directions:
            dfs(r + dr, c + dc, queue)
    
    # Find first island using DFS
    queue = deque()
    found = False
    for r in range(n):
        if found:
            break
        for c in range(n):
            if grid[r][c] == 1:
                dfs(r, c, queue)
                found = True
                break
    
    # BFS to find shortest path to second island
    while queue:
        r, c, distance = queue.popleft()
        
        for dr, dc in directions:
            nr, nc = r + dr, c + dc
            
            if 0 <= nr < n and 0 <= nc < n:
                if grid[nr][nc] == 1:  # Found second island
                    return distance
                elif grid[nr][nc] == 0:  # Water
                    grid[nr][nc] = 2
                    queue.append((nr, nc, distance + 1))
    
    return -1
```

---

## Interview Tips

### 1. When to Use Queue

```python
# Use queue for:
- BFS traversal
- Level order processing
- Shortest path (unweighted)
- Task scheduling
- First come first serve
- Buffering
```

### 2. Queue vs Stack

```python
# Queue (FIFO): BFS, level order, shortest path
# Stack (LIFO): DFS, backtracking, expression evaluation

# BFS with queue
queue = deque([start])
while queue:
    node = queue.popleft()
    # process node
    # add neighbors to queue

# DFS with stack
stack = [start]
while stack:
    node = stack.pop()
    # process node
    # add neighbors to stack
```

### 3. Use collections.deque

```python
# ✅ Use deque for queue operations
from collections import deque
queue = deque()
queue.append(item)      # O(1)
queue.popleft()         # O(1)

# ❌ Don't use list for queue
queue = []
queue.append(item)      # O(1)
queue.pop(0)            # O(n) - slow!
```

### 4. BFS Template

```python
from collections import deque

def bfs(start):
    queue = deque([start])
    visited = {start}
    
    while queue:
        # Process current level
        level_size = len(queue)
        
        for _ in range(level_size):
            node = queue.popleft()
            
            # Process node
            
            # Add unvisited neighbors
            for neighbor in get_neighbors(node):
                if neighbor not in visited:
                    visited.add(neighbor)
                    queue.append(neighbor)
```

---

## Practice Problems

### Easy
1. [Implement Queue using Stacks](https://leetcode.com/problems/implement-queue-using-stacks/)
2. [Number of Recent Calls](https://leetcode.com/problems/number-of-recent-calls/)
3. [Design Circular Queue](https://leetcode.com/problems/design-circular-queue/)
4. [Time Needed to Buy Tickets](https://leetcode.com/problems/time-needed-to-buy-tickets/)

### Medium
1. [Moving Average from Data Stream](https://leetcode.com/problems/moving-average-from-data-stream/)
2. [Rotting Oranges](https://leetcode.com/problems/rotting-oranges/)
3. [Perfect Squares](https://leetcode.com/problems/perfect-squares/)
4. [Open the Lock](https://leetcode.com/problems/open-the-lock/)
5. [Shortest Bridge](https://leetcode.com/problems/shortest-bridge/)
6. [Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/)
7. [Design Hit Counter](https://leetcode.com/problems/design-hit-counter/)

### Hard
1. [Shortest Path in Binary Matrix](https://leetcode.com/problems/shortest-path-in-binary-matrix/)
2. [Bus Routes](https://leetcode.com/problems/bus-routes/)

---

## Summary

### Key Takeaways
- ✅ Queue = FIFO (First In, First Out)
- ✅ O(1) enqueue, dequeue with deque
- ✅ Perfect for BFS and level order
- ✅ Use for shortest path (unweighted)
- ✅ Deque allows operations on both ends

### Common Patterns
```python
# 1. BFS with queue
from collections import deque
queue = deque([start])
visited = {start}

while queue:
    node = queue.popleft()
    for neighbor in neighbors:
        if neighbor not in visited:
            visited.add(neighbor)
            queue.append(neighbor)

# 2. Level order with queue
queue = deque([root])
while queue:
    level_size = len(queue)
    for _ in range(level_size):
        node = queue.popleft()
        # process level

# 3. Sliding window with deque
dq = deque()
for i, val in enumerate(arr):
    while dq and condition:
        dq.pop()  # or popleft()
    dq.append(i)
```

---

**Next**: [Hash Tables →](../06-hash-tables/README.md)

**Happy Coding! 🚀**
