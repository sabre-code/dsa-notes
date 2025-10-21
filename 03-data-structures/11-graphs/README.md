# 🕸️ Graphs - Python DSA

> Model relationships and networks

---

## 📚 Table of Contents

1. [Introduction](#introduction)
2. [Graph Representations](#graph-representations)
3. [Graph Types](#graph-types)
4. [Graph Traversals](#graph-traversals)
5. [Common Algorithms](#common-algorithms)
6. [Classic Problems](#classic-problems)
7. [Interview Tips](#interview-tips)
8. [Practice Problems](#practice-problems)

---

## Introduction

**Graph** is a non-linear data structure consisting of vertices (nodes) and edges (connections).

### Key Characteristics
- ✅ **Model relationships** - Social networks, maps, dependencies
- ✅ **Flexible structure** - Can represent complex connections
- ✅ **Multiple algorithms** - DFS, BFS, Dijkstra, etc.
- ✅ **Real-world applications** - Navigation, recommendations, networks

### Graph Terminology
- **Vertex (Node)**: Point in graph
- **Edge**: Connection between vertices
- **Directed**: Edges have direction (A → B)
- **Undirected**: Edges bidirectional (A ↔ B)
- **Weighted**: Edges have values/costs
- **Degree**: Number of edges connected to vertex
- **Path**: Sequence of vertices
- **Cycle**: Path that starts and ends at same vertex
- **Connected**: Path exists between all vertex pairs

### Real-World Examples
- 🌐 Social networks (friends, followers)
- 🗺️ Maps and navigation (cities, roads)
- 🌐 Web pages (links)
- 🔌 Computer networks
- 📦 Package dependencies

---

## Graph Representations

### 1. Adjacency Matrix

2D array where `matrix[i][j] = 1` if edge exists from i to j.

```python
# Graph: 0 → 1, 0 → 2, 1 → 2

matrix = [
    [0, 1, 1],  # 0 connects to 1, 2
    [0, 0, 1],  # 1 connects to 2
    [0, 0, 0]   # 2 connects to nothing
]

# Pros: O(1) edge lookup
# Cons: O(V²) space
```

```python
class GraphMatrix:
    def __init__(self, vertices):
        self.V = vertices
        self.graph = [[0] * vertices for _ in range(vertices)]
    
    def add_edge(self, u, v, directed=True):
        """Add edge from u to v."""
        self.graph[u][v] = 1
        if not directed:
            self.graph[v][u] = 1
    
    def has_edge(self, u, v):
        """Check if edge exists."""
        return self.graph[u][v] == 1
    
    def get_neighbors(self, u):
        """Get all neighbors of u."""
        return [v for v in range(self.V) if self.graph[u][v] == 1]
```

### 2. Adjacency List

Dictionary/list where each vertex maps to list of neighbors.

```python
# Graph: 0 → 1, 0 → 2, 1 → 2

adj_list = {
    0: [1, 2],
    1: [2],
    2: []
}

# Pros: O(V + E) space, efficient for sparse graphs
# Cons: O(V) edge lookup
```

```python
class Graph:
    def __init__(self):
        self.graph = {}
    
    def add_vertex(self, vertex):
        """Add vertex to graph."""
        if vertex not in self.graph:
            self.graph[vertex] = []
    
    def add_edge(self, u, v, directed=True):
        """Add edge from u to v."""
        if u not in self.graph:
            self.graph[u] = []
        if v not in self.graph:
            self.graph[v] = []
        
        self.graph[u].append(v)
        if not directed:
            self.graph[v].append(u)
    
    def get_neighbors(self, vertex):
        """Get neighbors of vertex."""
        return self.graph.get(vertex, [])
    
    def __repr__(self):
        return str(self.graph)

# Test
g = Graph()
g.add_edge(0, 1)
g.add_edge(0, 2)
g.add_edge(1, 2)
print(g)  # {0: [1, 2], 1: [2], 2: []}
```

### 3. Edge List

List of all edges as tuples.

```python
# Graph: 0 → 1, 0 → 2, 1 → 2

edges = [(0, 1), (0, 2), (1, 2)]

# Pros: Simple, compact
# Cons: Slow neighbor lookup
```

### 4. Weighted Graph

```python
# Adjacency list with weights
weighted_graph = {
    'A': [('B', 4), ('C', 2)],
    'B': [('C', 1), ('D', 5)],
    'C': [('D', 8)],
    'D': []
}

# Or adjacency matrix
weighted_matrix = [
    [0, 4, 2, float('inf')],
    [float('inf'), 0, 1, 5],
    [float('inf'), float('inf'), 0, 8],
    [float('inf'), float('inf'), float('inf'), 0]
]
```

---

## Graph Types

### 1. Undirected Graph
Edges have no direction.

```
A --- B
|     |
C --- D
```

### 2. Directed Graph (Digraph)
Edges have direction.

```
A → B
↓   ↓
C → D
```

### 3. Weighted Graph
Edges have weights/costs.

```
A --5-- B
|       |
2       3
|       |
C --1-- D
```

### 4. Cyclic vs Acyclic
- **Cyclic**: Contains cycles
- **Acyclic**: No cycles (DAG = Directed Acyclic Graph)

### 5. Connected vs Disconnected
- **Connected**: Path exists between all vertices
- **Disconnected**: Some vertices unreachable

---

## Graph Traversals

### 1. Depth-First Search (DFS)

Explore as far as possible along each branch before backtracking.

```python
def dfs_recursive(graph, start, visited=None):
    """
    DFS using recursion.
    
    Time: O(V + E)
    Space: O(V)
    """
    if visited is None:
        visited = set()
    
    visited.add(start)
    print(start, end=' ')
    
    for neighbor in graph.get(start, []):
        if neighbor not in visited:
            dfs_recursive(graph, neighbor, visited)
    
    return visited

def dfs_iterative(graph, start):
    """
    DFS using stack.
    
    Time: O(V + E)
    Space: O(V)
    """
    visited = set()
    stack = [start]
    
    while stack:
        vertex = stack.pop()
        
        if vertex not in visited:
            visited.add(vertex)
            print(vertex, end=' ')
            
            # Add neighbors to stack (reverse for same order as recursive)
            for neighbor in reversed(graph.get(vertex, [])):
                if neighbor not in visited:
                    stack.append(neighbor)
    
    return visited

# Test
graph = {
    0: [1, 2],
    1: [3, 4],
    2: [5],
    3: [],
    4: [],
    5: []
}
dfs_recursive(graph, 0)  # 0 1 3 4 2 5
```

### 2. Breadth-First Search (BFS)

Explore all neighbors at current depth before moving deeper.

```python
from collections import deque

def bfs(graph, start):
    """
    BFS using queue.
    
    Time: O(V + E)
    Space: O(V)
    """
    visited = set([start])
    queue = deque([start])
    result = []
    
    while queue:
        vertex = queue.popleft()
        result.append(vertex)
        
        for neighbor in graph.get(vertex, []):
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append(neighbor)
    
    return result

# Test
print(bfs(graph, 0))  # [0, 1, 2, 3, 4, 5]
```

### 3. DFS vs BFS

| Feature | DFS | BFS |
|---------|-----|-----|
| Data Structure | Stack/Recursion | Queue |
| When to use | Paths, cycles, topological sort | Shortest path, levels |
| Memory | O(h) height | O(w) width |
| Complete | No (infinite paths) | Yes |
| Optimal | No | Yes (unweighted) |

---

## Common Algorithms

### 1. Detect Cycle

#### Undirected Graph

```python
def has_cycle_undirected(graph):
    """
    Detect cycle in undirected graph using DFS.
    
    Time: O(V + E)
    Space: O(V)
    """
    visited = set()
    
    def dfs(node, parent):
        visited.add(node)
        
        for neighbor in graph.get(node, []):
            if neighbor not in visited:
                if dfs(neighbor, node):
                    return True
            elif neighbor != parent:
                return True  # Back edge found
        
        return False
    
    # Check all components
    for vertex in graph:
        if vertex not in visited:
            if dfs(vertex, -1):
                return True
    
    return False
```

#### Directed Graph

```python
def has_cycle_directed(graph):
    """
    Detect cycle in directed graph using DFS.
    
    Time: O(V + E)
    Space: O(V)
    """
    WHITE, GRAY, BLACK = 0, 1, 2
    color = {node: WHITE for node in graph}
    
    def dfs(node):
        if color[node] == GRAY:
            return True  # Back edge (cycle)
        if color[node] == BLACK:
            return False  # Already processed
        
        color[node] = GRAY  # Mark as being processed
        
        for neighbor in graph.get(node, []):
            if dfs(neighbor):
                return True
        
        color[node] = BLACK  # Mark as done
        return False
    
    for vertex in graph:
        if color[vertex] == WHITE:
            if dfs(vertex):
                return True
    
    return False
```

### 2. Topological Sort (DAG)

```python
def topological_sort(graph):
    """
    Topological sort using DFS.
    
    Time: O(V + E)
    Space: O(V)
    """
    visited = set()
    stack = []
    
    def dfs(node):
        visited.add(node)
        
        for neighbor in graph.get(node, []):
            if neighbor not in visited:
                dfs(neighbor)
        
        stack.append(node)  # Add after visiting all descendants
    
    for vertex in graph:
        if vertex not in visited:
            dfs(vertex)
    
    return stack[::-1]  # Reverse for correct order

# Using Kahn's Algorithm (BFS)
def topological_sort_kahn(graph):
    """
    Time: O(V + E)
    Space: O(V)
    """
    from collections import deque, defaultdict
    
    # Calculate in-degrees
    in_degree = defaultdict(int)
    for node in graph:
        for neighbor in graph[node]:
            in_degree[neighbor] += 1
    
    # Queue with 0 in-degree nodes
    queue = deque([node for node in graph if in_degree[node] == 0])
    result = []
    
    while queue:
        node = queue.popleft()
        result.append(node)
        
        for neighbor in graph.get(node, []):
            in_degree[neighbor] -= 1
            if in_degree[neighbor] == 0:
                queue.append(neighbor)
    
    return result if len(result) == len(graph) else []  # [] if cycle
```

### 3. Shortest Path (Unweighted)

```python
from collections import deque

def shortest_path_bfs(graph, start, end):
    """
    Shortest path in unweighted graph.
    
    Time: O(V + E)
    Space: O(V)
    """
    if start == end:
        return [start]
    
    visited = {start}
    queue = deque([(start, [start])])
    
    while queue:
        node, path = queue.popleft()
        
        for neighbor in graph.get(node, []):
            if neighbor == end:
                return path + [neighbor]
            
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append((neighbor, path + [neighbor]))
    
    return []  # No path found
```

### 4. Connected Components

```python
def count_components(n, edges):
    """
    Count connected components.
    
    Time: O(V + E)
    Space: O(V + E)
    """
    # Build graph
    graph = {i: [] for i in range(n)}
    for u, v in edges:
        graph[u].append(v)
        graph[v].append(u)
    
    visited = set()
    count = 0
    
    def dfs(node):
        visited.add(node)
        for neighbor in graph[node]:
            if neighbor not in visited:
                dfs(neighbor)
    
    for node in range(n):
        if node not in visited:
            dfs(node)
            count += 1
    
    return count
```

### 5. Dijkstra's Algorithm (Shortest Path Weighted)

```python
import heapq

def dijkstra(graph, start):
    """
    Shortest paths from start to all vertices.
    
    Time: O((V + E) log V)
    Space: O(V)
    """
    distances = {node: float('inf') for node in graph}
    distances[start] = 0
    
    pq = [(0, start)]  # (distance, node)
    
    while pq:
        current_dist, current_node = heapq.heappop(pq)
        
        if current_dist > distances[current_node]:
            continue
        
        for neighbor, weight in graph.get(current_node, []):
            distance = current_dist + weight
            
            if distance < distances[neighbor]:
                distances[neighbor] = distance
                heapq.heappush(pq, (distance, neighbor))
    
    return distances

# Example:
graph = {
    'A': [('B', 4), ('C', 2)],
    'B': [('C', 1), ('D', 5)],
    'C': [('D', 8)],
    'D': []
}
print(dijkstra(graph, 'A'))
# {'A': 0, 'B': 4, 'C': 2, 'D': 9}
```

---

## Classic Problems

### 1. Number of Islands

```python
def num_islands(grid: list[list[str]]) -> int:
    """
    Count islands in 2D grid.
    
    Time: O(m * n)
    Space: O(m * n)
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
        
        # Visit all 4 directions
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

### 2. Clone Graph

```python
class Node:
    def __init__(self, val=0, neighbors=None):
        self.val = val
        self.neighbors = neighbors if neighbors else []

def clone_graph(node: Node) -> Node:
    """
    Deep copy of graph.
    
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
```

### 3. Course Schedule (Detect Cycle)

```python
def can_finish(num_courses: int, prerequisites: list[list[int]]) -> bool:
    """
    Check if all courses can be finished.
    
    Time: O(V + E)
    Space: O(V + E)
    """
    # Build graph
    graph = {i: [] for i in range(num_courses)}
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

### 4. Course Schedule II (Topological Sort)

```python
def find_order(num_courses: int, prerequisites: list[list[int]]) -> list[int]:
    """
    Return ordering of courses to take.
    
    Time: O(V + E)
    Space: O(V + E)
    """
    from collections import deque, defaultdict
    
    # Build graph and calculate in-degrees
    graph = defaultdict(list)
    in_degree = [0] * num_courses
    
    for course, prereq in prerequisites:
        graph[prereq].append(course)
        in_degree[course] += 1
    
    # Start with courses having no prerequisites
    queue = deque([i for i in range(num_courses) if in_degree[i] == 0])
    result = []
    
    while queue:
        course = queue.popleft()
        result.append(course)
        
        for next_course in graph[course]:
            in_degree[next_course] -= 1
            if in_degree[next_course] == 0:
                queue.append(next_course)
    
    return result if len(result) == num_courses else []
```

### 5. Pacific Atlantic Water Flow

```python
def pacific_atlantic(heights: list[list[int]]) -> list[list[int]]:
    """
    Find cells that can flow to both oceans.
    
    Time: O(m * n)
    Space: O(m * n)
    """
    if not heights:
        return []
    
    rows, cols = len(heights), len(heights[0])
    pacific = set()
    atlantic = set()
    
    def dfs(r, c, visited):
        visited.add((r, c))
        
        for dr, dc in [(0,1), (1,0), (0,-1), (-1,0)]:
            nr, nc = r + dr, c + dc
            
            if (0 <= nr < rows and 0 <= nc < cols and
                (nr, nc) not in visited and
                heights[nr][nc] >= heights[r][c]):
                dfs(nr, nc, visited)
    
    # Start from edges
    for r in range(rows):
        dfs(r, 0, pacific)
        dfs(r, cols - 1, atlantic)
    
    for c in range(cols):
        dfs(0, c, pacific)
        dfs(rows - 1, c, atlantic)
    
    return list(pacific & atlantic)
```

### 6. Word Ladder

```python
from collections import deque

def ladder_length(begin_word: str, end_word: str, word_list: list[str]) -> int:
    """
    Shortest transformation sequence.
    
    Time: O(m² * n) where m is word length, n is list size
    Space: O(m * n)
    """
    word_set = set(word_list)
    if end_word not in word_set:
        return 0
    
    queue = deque([(begin_word, 1)])
    visited = {begin_word}
    
    while queue:
        word, length = queue.popleft()
        
        if word == end_word:
            return length
        
        # Try all possible transformations
        for i in range(len(word)):
            for c in 'abcdefghijklmnopqrstuvwxyz':
                next_word = word[:i] + c + word[i+1:]
                
                if next_word in word_set and next_word not in visited:
                    visited.add(next_word)
                    queue.append((next_word, length + 1))
    
    return 0
```

### 7. Alien Dictionary (Topological Sort)

```python
def alien_order(words: list[str]) -> str:
    """
    Find order of characters in alien language.
    
    Time: O(C) where C is total characters
    Space: O(1) - at most 26 characters
    """
    from collections import defaultdict, deque
    
    # Build graph
    graph = defaultdict(set)
    in_degree = {c: 0 for word in words for c in word}
    
    # Add edges from adjacent words
    for i in range(len(words) - 1):
        word1, word2 = words[i], words[i + 1]
        min_len = min(len(word1), len(word2))
        
        # Invalid: word1 is prefix of word2 but longer
        if len(word1) > len(word2) and word1[:min_len] == word2[:min_len]:
            return ""
        
        for j in range(min_len):
            if word1[j] != word2[j]:
                if word2[j] not in graph[word1[j]]:
                    graph[word1[j]].add(word2[j])
                    in_degree[word2[j]] += 1
                break
    
    # Topological sort
    queue = deque([c for c in in_degree if in_degree[c] == 0])
    result = []
    
    while queue:
        c = queue.popleft()
        result.append(c)
        
        for neighbor in graph[c]:
            in_degree[neighbor] -= 1
            if in_degree[neighbor] == 0:
                queue.append(neighbor)
    
    return ''.join(result) if len(result) == len(in_degree) else ""
```

### 8. Network Delay Time (Dijkstra)

```python
import heapq

def network_delay_time(times: list[list[int]], n: int, k: int) -> int:
    """
    Time for signal to reach all nodes.
    
    Time: O((V + E) log V)
    Space: O(V + E)
    """
    # Build graph
    graph = {i: [] for i in range(1, n + 1)}
    for u, v, w in times:
        graph[u].append((v, w))
    
    # Dijkstra's algorithm
    distances = {i: float('inf') for i in range(1, n + 1)}
    distances[k] = 0
    
    pq = [(0, k)]
    
    while pq:
        time, node = heapq.heappop(pq)
        
        if time > distances[node]:
            continue
        
        for neighbor, weight in graph[node]:
            new_time = time + weight
            
            if new_time < distances[neighbor]:
                distances[neighbor] = new_time
                heapq.heappush(pq, (new_time, neighbor))
    
    max_time = max(distances.values())
    return max_time if max_time != float('inf') else -1
```

### 9. Min Cost to Connect All Points (MST)

```python
import heapq

def min_cost_connect_points(points: list[list[int]]) -> int:
    """
    Minimum spanning tree using Prim's algorithm.
    
    Time: O(n² log n)
    Space: O(n²)
    """
    n = len(points)
    visited = set([0])
    
    # Min heap of (cost, point_index)
    heap = []
    for i in range(1, n):
        cost = abs(points[0][0] - points[i][0]) + abs(points[0][1] - points[i][1])
        heapq.heappush(heap, (cost, i))
    
    total_cost = 0
    
    while len(visited) < n:
        cost, i = heapq.heappop(heap)
        
        if i in visited:
            continue
        
        visited.add(i)
        total_cost += cost
        
        for j in range(n):
            if j not in visited:
                new_cost = abs(points[i][0] - points[j][0]) + abs(points[i][1] - points[j][1])
                heapq.heappush(heap, (new_cost, j))
    
    return total_cost
```

### 10. Cheapest Flights Within K Stops

```python
from collections import defaultdict, deque

def find_cheapest_price(n: int, flights: list[list[int]], src: int, dst: int, k: int) -> int:
    """
    BFS with limited stops.
    
    Time: O(E * k)
    Space: O(V + E)
    """
    # Build graph
    graph = defaultdict(list)
    for u, v, price in flights:
        graph[u].append((v, price))
    
    # BFS
    queue = deque([(src, 0, 0)])  # (node, cost, stops)
    min_cost = {src: 0}
    
    while queue:
        node, cost, stops = queue.popleft()
        
        if stops > k:
            continue
        
        for neighbor, price in graph[node]:
            new_cost = cost + price
            
            # Only add if cheaper OR fewer stops
            if neighbor not in min_cost or new_cost < min_cost[neighbor]:
                min_cost[neighbor] = new_cost
                queue.append((neighbor, new_cost, stops + 1))
    
    return min_cost.get(dst, -1)
```

---

## Interview Tips

### 1. When to Use DFS vs BFS

```python
# Use DFS for:
- Detecting cycles
- Topological sort
- Path finding (all paths)
- Connected components
- Tree problems

# Use BFS for:
- Shortest path (unweighted)
- Level-order problems
- Minimum steps
- Closest nodes
```

### 2. Graph Representation Choice

```python
# Adjacency List (most common):
- Space: O(V + E)
- Good for sparse graphs
- Fast neighbor iteration

# Adjacency Matrix:
- Space: O(V²)
- Good for dense graphs
- O(1) edge lookup

# Edge List:
- Space: O(E)
- Simple, but slow operations
```

### 3. Common Graph Patterns

```python
# 1. DFS template
def dfs(graph, node, visited):
    visited.add(node)
    for neighbor in graph[node]:
        if neighbor not in visited:
            dfs(graph, neighbor, visited)

# 2. BFS template
from collections import deque
queue = deque([start])
visited = {start}
while queue:
    node = queue.popleft()
    for neighbor in graph[node]:
        if neighbor not in visited:
            visited.add(neighbor)
            queue.append(neighbor)

# 3. Grid as graph (4 directions)
for dr, dc in [(0,1), (1,0), (0,-1), (-1,0)]:
    nr, nc = r + dr, c + dc
    if 0 <= nr < rows and 0 <= nc < cols:
        # Process neighbor
```

### 4. Detect Cycles

```python
# Undirected: Check if visiting non-parent visited node
def has_cycle(graph, node, parent, visited):
    visited.add(node)
    for neighbor in graph[node]:
        if neighbor not in visited:
            if has_cycle(graph, neighbor, node, visited):
                return True
        elif neighbor != parent:
            return True
    return False

# Directed: Use 3 colors (WHITE, GRAY, BLACK)
WHITE, GRAY, BLACK = 0, 1, 2
def has_cycle(node):
    if color[node] == GRAY:
        return True  # Back edge
    if color[node] == BLACK:
        return False
    
    color[node] = GRAY
    for neighbor in graph[node]:
        if has_cycle(neighbor):
            return True
    color[node] = BLACK
    return False
```

---

## Practice Problems

### Easy
1. [Find Center of Star Graph](https://leetcode.com/problems/find-center-of-star-graph/)
2. [Find if Path Exists in Graph](https://leetcode.com/problems/find-if-path-exists-in-graph/)
3. [Number of Provinces](https://leetcode.com/problems/number-of-provinces/)

### Medium
1. [Number of Islands](https://leetcode.com/problems/number-of-islands/)
2. [Clone Graph](https://leetcode.com/problems/clone-graph/)
3. [Course Schedule](https://leetcode.com/problems/course-schedule/)
4. [Course Schedule II](https://leetcode.com/problems/course-schedule-ii/)
5. [Pacific Atlantic Water Flow](https://leetcode.com/problems/pacific-atlantic-water-flow/)
6. [Number of Connected Components](https://leetcode.com/problems/number-of-connected-components-in-an-undirected-graph/)
7. [Graph Valid Tree](https://leetcode.com/problems/graph-valid-tree/)
8. [Word Ladder](https://leetcode.com/problems/word-ladder/)
9. [Rotting Oranges](https://leetcode.com/problems/rotting-oranges/)

### Hard
1. [Alien Dictionary](https://leetcode.com/problems/alien-dictionary/)
2. [Network Delay Time](https://leetcode.com/problems/network-delay-time/)
3. [Min Cost to Connect All Points](https://leetcode.com/problems/min-cost-to-connect-all-points/)
4. [Reconstruct Itinerary](https://leetcode.com/problems/reconstruct-itinerary/)
5. [Swim in Rising Water](https://leetcode.com/problems/swim-in-rising-water/)

---

## Summary

### Key Takeaways
- ✅ Graph = Vertices + Edges
- ✅ DFS for cycles, paths, components
- ✅ BFS for shortest path (unweighted)
- ✅ Topological sort for DAGs
- ✅ Dijkstra for weighted shortest path

### Common Patterns
```python
# 1. DFS recursive
visited = set()
def dfs(node):
    visited.add(node)
    for neighbor in graph[node]:
        if neighbor not in visited:
            dfs(neighbor)

# 2. BFS with queue
from collections import deque
queue = deque([start])
visited = {start}
while queue:
    node = queue.popleft()
    for neighbor in graph[node]:
        if neighbor not in visited:
            visited.add(neighbor)
            queue.append(neighbor)

# 3. Grid DFS/BFS
directions = [(0,1), (1,0), (0,-1), (-1,0)]
def dfs(r, c):
    if not (0 <= r < rows and 0 <= c < cols):
        return
    for dr, dc in directions:
        dfs(r + dr, c + dc)

# 4. Topological sort
def topo_sort():
    in_degree = calculate_in_degrees()
    queue = [node for node in graph if in_degree[node] == 0]
    result = []
    while queue:
        node = queue.pop(0)
        result.append(node)
        for neighbor in graph[node]:
            in_degree[neighbor] -= 1
            if in_degree[neighbor] == 0:
                queue.append(neighbor)
    return result
```

---

**Congratulations! You've completed all Data Structures! 🎉**

**Next**: [Algorithm Patterns →](../../04-algorithm-patterns/README.md)

**Happy Coding! 🚀**
