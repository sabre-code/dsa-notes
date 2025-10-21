# 🗺️ Graph Algorithms - Python DSA

> Advanced graph algorithms for interviews

---

## 📚 Table of Contents

1. [Introduction](#introduction)
2. [Shortest Path Algorithms](#shortest-path-algorithms)
3. [Minimum Spanning Tree](#minimum-spanning-tree)
4. [Union-Find](#union-find)
5. [Advanced Problems](#advanced-problems)
6. [Interview Tips](#interview-tips)
7. [Practice Problems](#practice-problems)

---

## Introduction

**Graph Algorithms** solve complex problems on graphs beyond basic DFS/BFS.

### Key Algorithms
- ✅ **Dijkstra** - Shortest path (weighted, non-negative)
- ✅ **Bellman-Ford** - Shortest path (negative edges)
- ✅ **Floyd-Warshall** - All-pairs shortest path
- ✅ **Kruskal/Prim** - Minimum spanning tree
- ✅ **Union-Find** - Disjoint sets

---

## Shortest Path Algorithms

### 1. Dijkstra's Algorithm

```python
import heapq

def dijkstra(graph: dict, start: str) -> dict:
    """
    Shortest paths from start to all vertices.
    
    Time: O((V + E) log V)
    Space: O(V)
    
    Example:
    graph = {
        'A': [('B', 4), ('C', 2)],
        'B': [('C', 1), ('D', 5)],
        'C': [('D', 8)],
        'D': []
    }
    Output: {'A': 0, 'B': 4, 'C': 2, 'D': 9}
    """
    distances = {node: float('inf') for node in graph}
    distances[start] = 0
    
    pq = [(0, start)]  # (distance, node)
    
    while pq:
        curr_dist, curr_node = heapq.heappop(pq)
        
        if curr_dist > distances[curr_node]:
            continue
        
        for neighbor, weight in graph.get(curr_node, []):
            distance = curr_dist + weight
            
            if distance < distances[neighbor]:
                distances[neighbor] = distance
                heapq.heappush(pq, (distance, neighbor))
    
    return distances

# Test
graph = {
    'A': [('B', 4), ('C', 2)],
    'B': [('C', 1), ('D', 5)],
    'C': [('D', 8)],
    'D': []
}
print(dijkstra(graph, 'A'))
# {'A': 0, 'B': 4, 'C': 2, 'D': 9}
```

### 2. Dijkstra with Path Reconstruction

```python
import heapq

def dijkstra_with_path(graph: dict, start: str, end: str) -> tuple:
    """
    Find shortest path and distance.
    
    Time: O((V + E) log V)
    Space: O(V)
    """
    distances = {node: float('inf') for node in graph}
    distances[start] = 0
    parent = {start: None}
    
    pq = [(0, start)]
    
    while pq:
        curr_dist, curr_node = heapq.heappop(pq)
        
        if curr_node == end:
            break
        
        if curr_dist > distances[curr_node]:
            continue
        
        for neighbor, weight in graph.get(curr_node, []):
            distance = curr_dist + weight
            
            if distance < distances[neighbor]:
                distances[neighbor] = distance
                parent[neighbor] = curr_node
                heapq.heappush(pq, (distance, neighbor))
    
    # Reconstruct path
    path = []
    node = end
    while node is not None:
        path.append(node)
        node = parent.get(node)
    path.reverse()
    
    return distances[end], path

# Test
print(dijkstra_with_path(graph, 'A', 'D'))
# (9, ['A', 'C', 'D']) or (9, ['A', 'B', 'D'])
```

### 3. Bellman-Ford Algorithm

```python
def bellman_ford(n: int, edges: list[tuple], start: int) -> list:
    """
    Shortest paths with negative edges.
    
    Time: O(V * E)
    Space: O(V)
    
    Example:
    n = 4, edges = [(0,1,5), (0,2,4), (1,3,3), (2,1,-6)]
    Returns: [0, -2, 4, 1]
    """
    distances = [float('inf')] * n
    distances[start] = 0
    
    # Relax edges V-1 times
    for _ in range(n - 1):
        for u, v, weight in edges:
            if distances[u] != float('inf'):
                distances[v] = min(distances[v], distances[u] + weight)
    
    # Check for negative cycles
    for u, v, weight in edges:
        if distances[u] != float('inf'):
            if distances[u] + weight < distances[v]:
                return None  # Negative cycle exists
    
    return distances

# Test
edges = [(0,1,5), (0,2,4), (1,3,3), (2,1,-6)]
print(bellman_ford(4, edges, 0))
# [0, -2, 4, 1]
```

### 4. Floyd-Warshall Algorithm

```python
def floyd_warshall(n: int, edges: list[tuple]) -> list[list]:
    """
    All-pairs shortest paths.
    
    Time: O(V³)
    Space: O(V²)
    
    Example:
    n = 4, edges = [(0,1,3), (1,2,1), (0,2,7), (2,3,2)]
    """
    # Initialize distance matrix
    dist = [[float('inf')] * n for _ in range(n)]
    
    for i in range(n):
        dist[i][i] = 0
    
    for u, v, weight in edges:
        dist[u][v] = weight
    
    # Floyd-Warshall: try all intermediate vertices
    for k in range(n):
        for i in range(n):
            for j in range(n):
                dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j])
    
    return dist

# Test
edges = [(0,1,3), (1,2,1), (0,2,7), (2,3,2)]
print(floyd_warshall(4, edges))
```

---

## Minimum Spanning Tree

### 1. Kruskal's Algorithm

```python
class UnionFind:
    def __init__(self, n):
        self.parent = list(range(n))
        self.rank = [0] * n
    
    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]
    
    def union(self, x, y):
        px, py = self.find(x), self.find(y)
        if px == py:
            return False
        if self.rank[px] < self.rank[py]:
            px, py = py, px
        self.parent[py] = px
        if self.rank[px] == self.rank[py]:
            self.rank[px] += 1
        return True

def kruskal(n: int, edges: list[tuple]) -> tuple:
    """
    Minimum spanning tree using Kruskal's.
    
    Time: O(E log E)
    Space: O(V)
    
    Example:
    n = 4, edges = [(0,1,10), (0,2,6), (0,3,5), (1,3,15), (2,3,4)]
    Returns: (15, [(2,3,4), (0,3,5), (0,2,6)])
    """
    # Sort edges by weight
    edges.sort(key=lambda x: x[2])
    
    uf = UnionFind(n)
    mst = []
    total_cost = 0
    
    for u, v, weight in edges:
        if uf.union(u, v):
            mst.append((u, v, weight))
            total_cost += weight
            
            if len(mst) == n - 1:
                break
    
    return total_cost, mst

# Test
edges = [(0,1,10), (0,2,6), (0,3,5), (1,3,15), (2,3,4)]
print(kruskal(4, edges))
# (15, [(2, 3, 4), (0, 3, 5), (0, 1, 10)])
```

### 2. Prim's Algorithm

```python
import heapq

def prim(n: int, graph: dict) -> tuple:
    """
    Minimum spanning tree using Prim's.
    
    Time: O((V + E) log V)
    Space: O(V)
    
    Example:
    graph = {
        0: [(1,10), (2,6), (3,5)],
        1: [(0,10), (3,15)],
        2: [(0,6), (3,4)],
        3: [(0,5), (1,15), (2,4)]
    }
    """
    visited = set([0])
    mst = []
    total_cost = 0
    
    # Min heap of (weight, from, to)
    pq = [(weight, 0, neighbor) for neighbor, weight in graph[0]]
    heapq.heapify(pq)
    
    while pq and len(visited) < n:
        weight, u, v = heapq.heappop(pq)
        
        if v in visited:
            continue
        
        visited.add(v)
        mst.append((u, v, weight))
        total_cost += weight
        
        for neighbor, w in graph[v]:
            if neighbor not in visited:
                heapq.heappush(pq, (w, v, neighbor))
    
    return total_cost, mst

# Test
graph = {
    0: [(1,10), (2,6), (3,5)],
    1: [(0,10), (3,15)],
    2: [(0,6), (3,4)],
    3: [(0,5), (1,15), (2,4)]
}
print(prim(4, graph))
```

---

## Union-Find

### Union-Find (Disjoint Set Union)

```python
class UnionFind:
    """
    Union-Find with path compression and union by rank.
    
    Time: O(α(n)) ≈ O(1) per operation
    Space: O(n)
    """
    def __init__(self, n: int):
        self.parent = list(range(n))
        self.rank = [0] * n
        self.components = n
    
    def find(self, x: int) -> int:
        """Find root with path compression."""
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]
    
    def union(self, x: int, y: int) -> bool:
        """Union by rank. Returns True if merged."""
        px, py = self.find(x), self.find(y)
        
        if px == py:
            return False
        
        if self.rank[px] < self.rank[py]:
            px, py = py, px
        
        self.parent[py] = px
        if self.rank[px] == self.rank[py]:
            self.rank[px] += 1
        
        self.components -= 1
        return True
    
    def connected(self, x: int, y: int) -> bool:
        """Check if x and y are in same component."""
        return self.find(x) == self.find(y)
    
    def count_components(self) -> int:
        """Return number of components."""
        return self.components

# Test
uf = UnionFind(5)
uf.union(0, 1)
uf.union(1, 2)
print(uf.connected(0, 2))  # True
print(uf.connected(0, 3))  # False
print(uf.count_components())  # 3
```

---

## Advanced Problems

### Problem 1: Network Delay Time

```python
import heapq

def network_delay_time(times: list[list[int]], n: int, k: int) -> int:
    """
    Time for signal to reach all nodes (Dijkstra).
    
    Time: O((V + E) log V)
    Space: O(V + E)
    
    Example: times = [[2,1,1],[2,3,1],[3,4,1]], n = 4, k = 2
    Output: 2
    """
    # Build graph
    graph = {i: [] for i in range(1, n + 1)}
    for u, v, w in times:
        graph[u].append((v, w))
    
    # Dijkstra
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

# Test
print(network_delay_time([[2,1,1],[2,3,1],[3,4,1]], 4, 2))  # 2
```

### Problem 2: Cheapest Flights Within K Stops

```python
from collections import defaultdict, deque

def find_cheapest_price(n: int, flights: list[list[int]], src: int, dst: int, k: int) -> int:
    """
    Cheapest flight with at most k stops (Modified BFS).
    
    Time: O(E * k)
    Space: O(V + E)
    
    Example: n = 4, flights = [[0,1,100],[1,2,100],[2,0,100],[1,3,600],[2,3,200]]
             src = 0, dst = 3, k = 1
    Output: 700
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
            
            if neighbor not in min_cost or new_cost < min_cost[neighbor]:
                min_cost[neighbor] = new_cost
                queue.append((neighbor, new_cost, stops + 1))
    
    return min_cost.get(dst, -1)

# Test
flights = [[0,1,100],[1,2,100],[2,0,100],[1,3,600],[2,3,200]]
print(find_cheapest_price(4, flights, 0, 3, 1))  # 700
```

### Problem 3: Number of Connected Components

```python
def count_components(n: int, edges: list[list[int]]) -> int:
    """
    Count connected components using Union-Find.
    
    Time: O(E * α(V))
    Space: O(V)
    
    Example: n = 5, edges = [[0,1],[1,2],[3,4]]
    Output: 2
    """
    uf = UnionFind(n)
    
    for u, v in edges:
        uf.union(u, v)
    
    return uf.count_components()

# Test
print(count_components(5, [[0,1],[1,2],[3,4]]))  # 2
```

### Problem 4: Graph Valid Tree

```python
def valid_tree(n: int, edges: list[list[int]]) -> bool:
    """
    Check if graph is valid tree (Union-Find).
    
    Time: O(E * α(V))
    Space: O(V)
    
    Example: n = 5, edges = [[0,1],[0,2],[0,3],[1,4]]
    Output: True
    """
    # Tree must have exactly n-1 edges and be connected
    if len(edges) != n - 1:
        return False
    
    uf = UnionFind(n)
    
    for u, v in edges:
        if not uf.union(u, v):
            return False  # Cycle detected
    
    return uf.count_components() == 1

# Test
print(valid_tree(5, [[0,1],[0,2],[0,3],[1,4]]))  # True
```

### Problem 5: Redundant Connection

```python
def find_redundant_connection(edges: list[list[int]]) -> list[int]:
    """
    Find edge that creates cycle (Union-Find).
    
    Time: O(E * α(V))
    Space: O(V)
    
    Example: edges = [[1,2],[1,3],[2,3]]
    Output: [2,3]
    """
    n = len(edges)
    uf = UnionFind(n + 1)
    
    for u, v in edges:
        if not uf.union(u, v):
            return [u, v]
    
    return []

# Test
print(find_redundant_connection([[1,2],[1,3],[2,3]]))  # [2,3]
```

### Problem 6: Accounts Merge

```python
def accounts_merge(accounts: list[list[str]]) -> list[list[str]]:
    """
    Merge accounts with common emails (Union-Find).
    
    Time: O(N * α(N))
    Space: O(N)
    
    Example:
    accounts = [["John","john@mail.com","john_work@mail.com"],
                ["John","john@mail.com","john@other.com"]]
    Output: [["John","john@mail.com","john@other.com","john_work@mail.com"]]
    """
    from collections import defaultdict
    
    email_to_id = {}
    email_to_name = {}
    
    # Assign unique ID to each email
    for i, account in enumerate(accounts):
        name = account[0]
        for email in account[1:]:
            email_to_name[email] = name
            if email not in email_to_id:
                email_to_id[email] = len(email_to_id)
    
    # Union-Find
    uf = UnionFind(len(email_to_id))
    
    for account in accounts:
        first_email_id = email_to_id[account[1]]
        for email in account[2:]:
            uf.union(first_email_id, email_to_id[email])
    
    # Group emails by root
    groups = defaultdict(list)
    for email, email_id in email_to_id.items():
        root = uf.find(email_id)
        groups[root].append(email)
    
    # Build result
    result = []
    for emails in groups.values():
        name = email_to_name[emails[0]]
        result.append([name] + sorted(emails))
    
    return result

# Test
accounts = [
    ["John","john@mail.com","john_work@mail.com"],
    ["John","john@mail.com","john@other.com"]
]
print(accounts_merge(accounts))
```

---

## Interview Tips

### 1. Algorithm Selection

```python
# Shortest Path - Single Source:
- Non-negative weights → Dijkstra O((V+E)logV)
- Negative weights → Bellman-Ford O(VE)
- Unweighted → BFS O(V+E)

# Shortest Path - All Pairs:
- Floyd-Warshall O(V³)
- Or run Dijkstra V times

# Minimum Spanning Tree:
- Kruskal (sparse graphs) O(E log E)
- Prim (dense graphs) O((V+E)logV)

# Connected Components:
- DFS/BFS O(V+E)
- Union-Find O(E * α(V))
```

### 2. Common Patterns

```python
# Pattern 1: Dijkstra Template
pq = [(0, start)]
distances = {start: 0}
while pq:
    dist, node = heapq.heappop(pq)
    if dist > distances[node]:
        continue
    for neighbor, weight in graph[node]:
        new_dist = dist + weight
        if new_dist < distances.get(neighbor, inf):
            distances[neighbor] = new_dist
            heapq.heappush(pq, (new_dist, neighbor))

# Pattern 2: Union-Find Template
class UnionFind:
    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]
    
    def union(self, x, y):
        px, py = self.find(x), self.find(y)
        if px == py:
            return False
        self.parent[py] = px
        return True
```

### 3. Optimization Tips

```python
# Dijkstra optimizations:
✅ Use min heap (priority queue)
✅ Check if distance improved before processing
✅ Can terminate early if destination reached

# Union-Find optimizations:
✅ Path compression in find()
✅ Union by rank/size
✅ Both give O(α(n)) ≈ O(1)
```

---

## Practice Problems

### Medium
1. [Network Delay Time](https://leetcode.com/problems/network-delay-time/)
2. [Cheapest Flights Within K Stops](https://leetcode.com/problems/cheapest-flights-within-k-stops/)
3. [Number of Connected Components](https://leetcode.com/problems/number-of-connected-components-in-an-undirected-graph/)
4. [Graph Valid Tree](https://leetcode.com/problems/graph-valid-tree/)
5. [Redundant Connection](https://leetcode.com/problems/redundant-connection/)
6. [Min Cost to Connect All Points](https://leetcode.com/problems/min-cost-to-connect-all-points/)

### Hard
1. [Accounts Merge](https://leetcode.com/problems/accounts-merge/)
2. [Critical Connections in a Network](https://leetcode.com/problems/critical-connections-in-a-network/)
3. [Swim in Rising Water](https://leetcode.com/problems/swim-in-rising-water/)

---

## Summary

### Key Takeaways
- ✅ **Dijkstra** - Single-source shortest path (non-negative)
- ✅ **Bellman-Ford** - Handles negative weights
- ✅ **Floyd-Warshall** - All-pairs shortest path
- ✅ **Kruskal/Prim** - Minimum spanning tree
- ✅ **Union-Find** - Connected components, cycles

### Quick Reference

```python
# Dijkstra
pq = [(0, start)]
while pq:
    dist, node = heapq.heappop(pq)
    for neighbor, weight in graph[node]:
        heapq.heappush(pq, (dist + weight, neighbor))

# Union-Find
uf = UnionFind(n)
for u, v in edges:
    uf.union(u, v)
```

---

**Next**: [Bit Manipulation →](../08-bit-manipulation/README.md)

**Happy Coding! 🚀**
