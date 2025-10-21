# 🔺 Heaps & Priority Queues - Python DSA

> Efficient priority-based data structure

---

## 📚 Table of Contents

1. [Introduction](#introduction)
2. [Heap Properties](#heap-properties)
3. [Implementation](#implementation)
4. [Common Operations](#common-operations)
5. [Common Patterns](#common-patterns)
6. [Classic Problems](#classic-problems)
7. [Interview Tips](#interview-tips)
8. [Practice Problems](#practice-problems)

---

## Introduction

**Heap** is a complete binary tree that satisfies the heap property. **Priority Queue** is an abstract data type implemented using heaps.

### Key Characteristics
- ✅ **O(1) access to min/max** - Constant time peek
- ✅ **O(log n) insertion** - Efficient add
- ✅ **O(log n) deletion** - Efficient remove
- ✅ **Complete binary tree** - Efficient array representation
- ✅ **Top K problems** - Perfect for finding extremes

### Real-World Examples
- 🏥 Emergency room priority
- 📧 Email priority inbox
- 🖥️ Task scheduling (OS)
- 🎮 Game event processing
- 🚦 Dijkstra's shortest path

### Heap Operations

| Operation | Time | Description |
|-----------|------|-------------|
| peek() | O(1) | View min/max |
| push() | O(log n) | Insert element |
| pop() | O(log n) | Remove min/max |
| heapify() | O(n) | Build heap from array |

---

## Heap Properties

### Min Heap
Parent ≤ Children (smallest at root)

```
       1
      / \
     3   2
    / \ / \
   7  5 6  4

Array: [1, 3, 2, 7, 5, 6, 4]
```

### Max Heap
Parent ≥ Children (largest at root)

```
       9
      / \
     7   8
    / \ / \
   3  5 6  4

Array: [9, 7, 8, 3, 5, 6, 4]
```

### Heap as Array

```python
# For element at index i:
left_child = 2 * i + 1
right_child = 2 * i + 2
parent = (i - 1) // 2

# Example: Element at index 1
# left_child = 2*1 + 1 = 3
# right_child = 2*1 + 2 = 4
# parent = (1-1) // 2 = 0
```

---

## Implementation

### Using Python's heapq (Min Heap)

```python
import heapq

# Create empty heap
heap = []

# Push elements
heapq.heappush(heap, 5)
heapq.heappush(heap, 3)
heapq.heappush(heap, 7)
heapq.heappush(heap, 1)

# Peek minimum
print(heap[0])  # 1

# Pop minimum
min_val = heapq.heappop(heap)  # 1
print(min_val)

# Heapify from list
nums = [5, 3, 7, 1, 9, 2]
heapq.heapify(nums)  # O(n)
print(nums)  # [1, 3, 2, 5, 9, 7]

# Push and pop in one operation
heapq.heappushpop(heap, 4)  # Push 4, pop min

# Replace top
heapq.heapreplace(heap, 6)  # Pop min, push 6

# N largest/smallest
nums = [1, 8, 3, 5, 2, 7, 4]
print(heapq.nlargest(3, nums))   # [8, 7, 5]
print(heapq.nsmallest(3, nums))  # [1, 2, 3]
```

### Max Heap using heapq

```python
import heapq

# Negate values for max heap
max_heap = []

# Push
heapq.heappush(max_heap, -5)
heapq.heappush(max_heap, -3)
heapq.heappush(max_heap, -7)

# Pop (negate back)
max_val = -heapq.heappop(max_heap)  # 7
print(max_val)

# Or use wrapper class
class MaxHeap:
    def __init__(self):
        self.heap = []
    
    def push(self, val):
        heapq.heappush(self.heap, -val)
    
    def pop(self):
        return -heapq.heappop(self.heap)
    
    def peek(self):
        return -self.heap[0] if self.heap else None
    
    def __len__(self):
        return len(self.heap)
```

### Custom MinHeap Implementation

```python
class MinHeap:
    """Min heap from scratch."""
    
    def __init__(self):
        self.heap = []
    
    def push(self, val):
        """Insert element. Time: O(log n)"""
        self.heap.append(val)
        self._bubble_up(len(self.heap) - 1)
    
    def pop(self):
        """Remove and return minimum. Time: O(log n)"""
        if not self.heap:
            raise IndexError("Heap is empty")
        
        if len(self.heap) == 1:
            return self.heap.pop()
        
        # Replace root with last element
        min_val = self.heap[0]
        self.heap[0] = self.heap.pop()
        self._bubble_down(0)
        
        return min_val
    
    def peek(self):
        """View minimum. Time: O(1)"""
        if not self.heap:
            raise IndexError("Heap is empty")
        return self.heap[0]
    
    def _bubble_up(self, index):
        """Move element up to maintain heap property."""
        parent = (index - 1) // 2
        
        if index > 0 and self.heap[index] < self.heap[parent]:
            self.heap[index], self.heap[parent] = self.heap[parent], self.heap[index]
            self._bubble_up(parent)
    
    def _bubble_down(self, index):
        """Move element down to maintain heap property."""
        smallest = index
        left = 2 * index + 1
        right = 2 * index + 2
        
        if left < len(self.heap) and self.heap[left] < self.heap[smallest]:
            smallest = left
        
        if right < len(self.heap) and self.heap[right] < self.heap[smallest]:
            smallest = right
        
        if smallest != index:
            self.heap[index], self.heap[smallest] = self.heap[smallest], self.heap[index]
            self._bubble_down(smallest)
    
    def __len__(self):
        return len(self.heap)
    
    def __repr__(self):
        return f"MinHeap({self.heap})"

# Test
heap = MinHeap()
for val in [5, 3, 7, 1, 9, 2]:
    heap.push(val)

print(heap)  # MinHeap([1, 3, 2, 5, 9, 7])
print(heap.pop())  # 1
print(heap.peek())  # 2
```

### Priority Queue with Custom Objects

```python
import heapq

class Task:
    def __init__(self, name, priority):
        self.name = name
        self.priority = priority
    
    def __lt__(self, other):
        return self.priority < other.priority
    
    def __repr__(self):
        return f"Task({self.name}, {self.priority})"

# Use with heapq
pq = []
heapq.heappush(pq, Task("Email", 2))
heapq.heappush(pq, Task("Meeting", 1))
heapq.heappush(pq, Task("Call", 3))

print(heapq.heappop(pq))  # Task(Meeting, 1)

# Alternative: Use tuples
pq = []
heapq.heappush(pq, (2, "Email"))
heapq.heappush(pq, (1, "Meeting"))
heapq.heappush(pq, (3, "Call"))

print(heapq.heappop(pq))  # (1, 'Meeting')
```

---

## Common Operations

### Heapify Array

```python
import heapq

def heapify_min(arr):
    """
    Convert array to min heap.
    Time: O(n)
    Space: O(1)
    """
    heapq.heapify(arr)
    return arr

# Example
nums = [5, 3, 7, 1, 9, 2]
heapq.heapify(nums)
print(nums)  # [1, 3, 2, 5, 9, 7]
```

### Heap Sort

```python
import heapq

def heap_sort(arr):
    """
    Sort using heap.
    Time: O(n log n)
    Space: O(n)
    """
    heap = arr[:]
    heapq.heapify(heap)
    
    result = []
    while heap:
        result.append(heapq.heappop(heap))
    
    return result

# In-place heap sort
def heap_sort_inplace(arr):
    """
    Time: O(n log n)
    Space: O(1)
    """
    n = len(arr)
    
    # Build max heap
    for i in range(n // 2 - 1, -1, -1):
        heapify_down(arr, n, i)
    
    # Extract elements
    for i in range(n - 1, 0, -1):
        arr[0], arr[i] = arr[i], arr[0]
        heapify_down(arr, i, 0)
    
    return arr

def heapify_down(arr, n, i):
    largest = i
    left = 2 * i + 1
    right = 2 * i + 2
    
    if left < n and arr[left] > arr[largest]:
        largest = left
    if right < n and arr[right] > arr[largest]:
        largest = right
    
    if largest != i:
        arr[i], arr[largest] = arr[largest], arr[i]
        heapify_down(arr, n, largest)
```

---

## Common Patterns

### Pattern 1: Top K Elements

```python
import heapq

def top_k_frequent(nums: list[int], k: int) -> list[int]:
    """
    Find k most frequent elements.
    
    Time: O(n log k)
    Space: O(n)
    """
    from collections import Counter
    
    count = Counter(nums)
    
    # Use min heap of size k
    heap = []
    for num, freq in count.items():
        heapq.heappush(heap, (freq, num))
        if len(heap) > k:
            heapq.heappop(heap)
    
    return [num for freq, num in heap]

# Alternative: Using nlargest
def top_k_frequent_v2(nums: list[int], k: int) -> list[int]:
    """
    Time: O(n log k)
    Space: O(n)
    """
    from collections import Counter
    count = Counter(nums)
    return heapq.nlargest(k, count.keys(), key=count.get)
```

### Pattern 2: Merge K Sorted Lists/Arrays

```python
import heapq

def merge_k_sorted(lists: list[list[int]]) -> list[int]:
    """
    Merge k sorted arrays.
    
    Time: O(n log k) where n is total elements
    Space: O(k)
    """
    heap = []
    
    # Add first element from each list
    for i, lst in enumerate(lists):
        if lst:
            heapq.heappush(heap, (lst[0], i, 0))
    
    result = []
    
    while heap:
        val, list_idx, elem_idx = heapq.heappop(heap)
        result.append(val)
        
        # Add next element from same list
        if elem_idx + 1 < len(lists[list_idx]):
            next_val = lists[list_idx][elem_idx + 1]
            heapq.heappush(heap, (next_val, list_idx, elem_idx + 1))
    
    return result
```

### Pattern 3: Running Median

```python
import heapq

class MedianFinder:
    """
    Find median from data stream.
    """
    
    def __init__(self):
        self.max_heap = []  # Left half (negated for max heap)
        self.min_heap = []  # Right half
    
    def add_num(self, num: int) -> None:
        """
        Time: O(log n)
        """
        # Add to max heap (left half)
        heapq.heappush(self.max_heap, -num)
        
        # Balance: move largest from left to right
        heapq.heappush(self.min_heap, -heapq.heappop(self.max_heap))
        
        # If right has more elements, move one to left
        if len(self.min_heap) > len(self.max_heap):
            heapq.heappush(self.max_heap, -heapq.heappop(self.min_heap))
    
    def find_median(self) -> float:
        """
        Time: O(1)
        """
        if len(self.max_heap) > len(self.min_heap):
            return -self.max_heap[0]
        return (-self.max_heap[0] + self.min_heap[0]) / 2

# Test
mf = MedianFinder()
mf.add_num(1)
mf.add_num(2)
print(mf.find_median())  # 1.5
mf.add_num(3)
print(mf.find_median())  # 2.0
```

---

## Classic Problems

### 1. Kth Largest Element

```python
import heapq

def find_kth_largest(nums: list[int], k: int) -> int:
    """
    Time: O(n log k)
    Space: O(k)
    """
    # Use min heap of size k
    heap = []
    
    for num in nums:
        heapq.heappush(heap, num)
        if len(heap) > k:
            heapq.heappop(heap)
    
    return heap[0]

# Alternative: Using nlargest
def find_kth_largest_v2(nums: list[int], k: int) -> int:
    """
    Time: O(n log k)
    Space: O(k)
    """
    return heapq.nlargest(k, nums)[-1]

# Alternative: Quick Select O(n) average
def find_kth_largest_quickselect(nums: list[int], k: int) -> int:
    """
    Time: O(n) average, O(n²) worst
    Space: O(1)
    """
    k = len(nums) - k  # Convert to kth smallest
    
    def quickselect(left, right):
        pivot = nums[right]
        p = left
        
        for i in range(left, right):
            if nums[i] <= pivot:
                nums[p], nums[i] = nums[i], nums[p]
                p += 1
        
        nums[p], nums[right] = nums[right], nums[p]
        
        if p < k:
            return quickselect(p + 1, right)
        elif p > k:
            return quickselect(left, p - 1)
        else:
            return nums[p]
    
    return quickselect(0, len(nums) - 1)
```

### 2. Top K Frequent Elements

```python
import heapq
from collections import Counter

def top_k_frequent(nums: list[int], k: int) -> list[int]:
    """
    Time: O(n log k)
    Space: O(n)
    """
    count = Counter(nums)
    return heapq.nlargest(k, count.keys(), key=count.get)
```

### 3. K Closest Points to Origin

```python
import heapq

def k_closest(points: list[list[int]], k: int) -> list[list[int]]:
    """
    Time: O(n log k)
    Space: O(k)
    """
    # Max heap (negate distances)
    heap = []
    
    for x, y in points:
        dist = -(x*x + y*y)  # Negate for max heap
        
        if len(heap) < k:
            heapq.heappush(heap, (dist, [x, y]))
        elif dist > heap[0][0]:
            heapq.heapreplace(heap, (dist, [x, y]))
    
    return [point for dist, point in heap]

# Alternative: Using nsmallest
def k_closest_v2(points: list[list[int]], k: int) -> list[list[int]]:
    """
    Time: O(n log k)
    Space: O(k)
    """
    return heapq.nsmallest(k, points, key=lambda p: p[0]**2 + p[1]**2)
```

### 4. Merge K Sorted Lists

```python
import heapq

def merge_k_lists(lists: list[ListNode]) -> ListNode:
    """
    Time: O(n log k)
    Space: O(k)
    """
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
```

### 5. Find Median from Data Stream

```python
import heapq

class MedianFinder:
    def __init__(self):
        self.max_heap = []  # Left half (smaller values)
        self.min_heap = []  # Right half (larger values)
    
    def add_num(self, num: int) -> None:
        """Time: O(log n)"""
        heapq.heappush(self.max_heap, -num)
        heapq.heappush(self.min_heap, -heapq.heappop(self.max_heap))
        
        if len(self.min_heap) > len(self.max_heap):
            heapq.heappush(self.max_heap, -heapq.heappop(self.min_heap))
    
    def find_median(self) -> float:
        """Time: O(1)"""
        if len(self.max_heap) > len(self.min_heap):
            return -self.max_heap[0]
        return (-self.max_heap[0] + self.min_heap[0]) / 2.0
```

### 6. Last Stone Weight

```python
import heapq

def last_stone_weight(stones: list[int]) -> int:
    """
    Time: O(n log n)
    Space: O(n)
    """
    # Max heap (negate values)
    heap = [-stone for stone in stones]
    heapq.heapify(heap)
    
    while len(heap) > 1:
        first = -heapq.heappop(heap)
        second = -heapq.heappop(heap)
        
        if first != second:
            heapq.heappush(heap, -(first - second))
    
    return -heap[0] if heap else 0
```

### 7. Kth Largest Element in Stream

```python
import heapq

class KthLargest:
    """
    Maintain kth largest element in stream.
    """
    
    def __init__(self, k: int, nums: list[int]):
        """Time: O(n log k)"""
        self.k = k
        self.heap = nums
        heapq.heapify(self.heap)
        
        # Keep only k largest
        while len(self.heap) > k:
            heapq.heappop(self.heap)
    
    def add(self, val: int) -> int:
        """Time: O(log k)"""
        heapq.heappush(self.heap, val)
        if len(self.heap) > self.k:
            heapq.heappop(self.heap)
        return self.heap[0]

# Test
kth_largest = KthLargest(3, [4, 5, 8, 2])
print(kth_largest.add(3))   # 4
print(kth_largest.add(5))   # 5
print(kth_largest.add(10))  # 5
```

### 8. Task Scheduler

```python
import heapq
from collections import Counter, deque

def least_interval(tasks: list[str], n: int) -> int:
    """
    Time: O(m) where m is total time
    Space: O(26) = O(1)
    """
    count = Counter(tasks)
    max_heap = [-cnt for cnt in count.values()]
    heapq.heapify(max_heap)
    
    time = 0
    queue = deque()  # (count, available_time)
    
    while max_heap or queue:
        time += 1
        
        if max_heap:
            cnt = heapq.heappop(max_heap) + 1
            if cnt < 0:
                queue.append((cnt, time + n))
        
        if queue and queue[0][1] == time:
            heapq.heappush(max_heap, queue.popleft()[0])
    
    return time
```

### 9. Meeting Rooms II

```python
import heapq

def min_meeting_rooms(intervals: list[list[int]]) -> int:
    """
    Find minimum meeting rooms needed.
    
    Time: O(n log n)
    Space: O(n)
    """
    if not intervals:
        return 0
    
    # Sort by start time
    intervals.sort(key=lambda x: x[0])
    
    # Min heap of end times
    heap = []
    heapq.heappush(heap, intervals[0][1])
    
    for start, end in intervals[1:]:
        # If earliest meeting ended, reuse room
        if start >= heap[0]:
            heapq.heappop(heap)
        
        # Add current meeting's end time
        heapq.heappush(heap, end)
    
    return len(heap)

# Example:
# intervals = [[0,30],[5,10],[15,20]]
# Output: 2
```

### 10. Reorganize String

```python
import heapq
from collections import Counter

def reorganize_string(s: str) -> str:
    """
    Rearrange so no two adjacent same characters.
    
    Time: O(n log k) where k is unique chars
    Space: O(k)
    """
    count = Counter(s)
    
    # Max heap
    max_heap = [(-cnt, char) for char, cnt in count.items()]
    heapq.heapify(max_heap)
    
    result = []
    prev_cnt, prev_char = 0, ''
    
    while max_heap:
        cnt, char = heapq.heappop(max_heap)
        result.append(char)
        
        # Add previous back if still has count
        if prev_cnt < 0:
            heapq.heappush(max_heap, (prev_cnt, prev_char))
        
        # Update previous
        prev_cnt, prev_char = cnt + 1, char
    
    result_str = ''.join(result)
    return result_str if len(result_str) == len(s) else ""

# Example:
# s = "aab" → "aba"
# s = "aaab" → "" (impossible)
```

### 11. Sliding Window Median

```python
import heapq

def median_sliding_window(nums: list[int], k: int) -> list[float]:
    """
    Time: O(n * k)
    Space: O(k)
    """
    result = []
    
    for i in range(len(nums) - k + 1):
        window = sorted(nums[i:i+k])
        
        if k % 2 == 1:
            result.append(float(window[k // 2]))
        else:
            result.append((window[k // 2 - 1] + window[k // 2]) / 2)
    
    return result

# Optimized with two heaps (complex, similar to MedianFinder)
```

### 12. Ugly Number II

```python
import heapq

def nth_ugly_number(n: int) -> int:
    """
    Ugly numbers have only prime factors 2, 3, 5.
    
    Time: O(n log n)
    Space: O(n)
    """
    heap = [1]
    seen = {1}
    factors = [2, 3, 5]
    
    for _ in range(n):
        num = heapq.heappop(heap)
        
        for factor in factors:
            new_num = num * factor
            if new_num not in seen:
                seen.add(new_num)
                heapq.heappush(heap, new_num)
    
    return num
```

---

## Interview Tips

### 1. When to Use Heap

```python
# Use heap for:
- Top K elements
- Kth largest/smallest
- Merge K sorted lists
- Running median
- Priority scheduling
- Closest points
- Meeting rooms
```

### 2. Min Heap vs Max Heap

```python
# Python heapq is MIN heap by default

# Min heap (default)
heap = []
heapq.heappush(heap, 5)
min_val = heapq.heappop(heap)

# Max heap (negate values)
max_heap = []
heapq.heappush(max_heap, -5)
max_val = -heapq.heappop(max_heap)

# Or use wrapper class
class MaxHeap:
    def __init__(self):
        self.heap = []
    def push(self, val):
        heapq.heappush(self.heap, -val)
    def pop(self):
        return -heapq.heappop(self.heap)
```

### 3. Top K Pattern

```python
# Use MIN heap of size k for TOP K LARGEST
heap = []
for num in nums:
    heapq.heappush(heap, num)
    if len(heap) > k:
        heapq.heappop(heap)  # Remove smallest

# Use MAX heap of size k for TOP K SMALLEST
max_heap = []
for num in nums:
    heapq.heappush(max_heap, -num)
    if len(max_heap) > k:
        heapq.heappop(max_heap)  # Remove largest
```

### 4. Two Heaps Pattern

```python
# For median: Use two heaps
# Max heap (left half) + Min heap (right half)
max_heap = []  # Smaller half
min_heap = []  # Larger half

# Keep max_heap.size = min_heap.size or max_heap.size = min_heap.size + 1
```

### 5. Common Heap Functions

```python
import heapq

# Create from list
heapq.heapify(arr)  # O(n)

# Push/pop
heapq.heappush(heap, item)     # O(log n)
heapq.heappop(heap)             # O(log n)

# Push and pop together
heapq.heappushpop(heap, item)  # O(log n)
heapq.heapreplace(heap, item)  # O(log n)

# N largest/smallest
heapq.nlargest(k, iterable)    # O(n log k)
heapq.nsmallest(k, iterable)   # O(n log k)

# Merge sorted iterables
heapq.merge(*iterables)        # O(n log k)
```

---

## Practice Problems

### Easy
1. [Last Stone Weight](https://leetcode.com/problems/last-stone-weight/)
2. [Kth Largest Element in Stream](https://leetcode.com/problems/kth-largest-element-in-a-stream/)
3. [Relative Ranks](https://leetcode.com/problems/relative-ranks/)
4. [Minimum Cost of Buying Candies](https://leetcode.com/problems/minimum-cost-of-buying-candies-with-discount/)

### Medium
1. [Kth Largest Element](https://leetcode.com/problems/kth-largest-element-in-an-array/)
2. [Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/)
3. [K Closest Points to Origin](https://leetcode.com/problems/k-closest-points-to-origin/)
4. [Reorganize String](https://leetcode.com/problems/reorganize-string/)
5. [Task Scheduler](https://leetcode.com/problems/task-scheduler/)
6. [Meeting Rooms II](https://leetcode.com/problems/meeting-rooms-ii/)
7. [Kth Smallest Element in Sorted Matrix](https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/)
8. [Ugly Number II](https://leetcode.com/problems/ugly-number-ii/)

### Hard
1. [Find Median from Data Stream](https://leetcode.com/problems/find-median-from-data-stream/)
2. [Merge K Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/)
3. [Sliding Window Median](https://leetcode.com/problems/sliding-window-median/)
4. [IPO](https://leetcode.com/problems/ipo/)

---

## Summary

### Key Takeaways
- ✅ Heap = Complete binary tree with heap property
- ✅ O(1) peek, O(log n) insert/delete
- ✅ Python heapq is MIN heap (negate for max)
- ✅ Perfect for Top K problems
- ✅ Two heaps pattern for median

### Common Patterns
```python
# 1. Top K with min heap
heap = []
for num in nums:
    heapq.heappush(heap, num)
    if len(heap) > k:
        heapq.heappop(heap)
return heap[0]  # Kth largest

# 2. Merge K sorted with heap
heap = []
for i, lst in enumerate(lists):
    if lst:
        heapq.heappush(heap, (lst[0], i, 0))

while heap:
    val, list_idx, elem_idx = heapq.heappop(heap)
    # Process and add next

# 3. Two heaps for median
max_heap = []  # Left half
min_heap = []  # Right half
# Keep balanced

# 4. Using nlargest/nsmallest
result = heapq.nlargest(k, nums)
result = heapq.nsmallest(k, nums)
```

---

**Next**: [Tries →](../10-tries/README.md)

**Happy Coding! 🚀**
