# 📅 Intervals - Python DSA

> Master interval merging, scheduling, and overlapping problems

---

## 📚 Table of Contents

1. [Introduction](#introduction)
2. [Pattern Recognition](#pattern-recognition)
3. [Merge Problems](#merge-problems)
4. [Scheduling Problems](#scheduling-problems)
5. [Overlap Detection](#overlap-detection)
6. [Interview Tips](#interview-tips)
7. [Practice Problems](#practice-problems)

---

## Introduction

**Intervals** involve working with ranges [start, end] for scheduling, merging, and overlap detection.

### Key Characteristics
- ✅ **Sort first** - Usually sort by start time
- ✅ **Overlap detection** - Check if intervals overlap
- ✅ **Greedy approach** - Process intervals in order
- ✅ **Common operations** - Merge, insert, remove

### When to Use
- **Scheduling** problems (meetings, tasks)
- **Merging** overlapping ranges
- **Finding** conflicts or gaps
- Keywords: "interval", "meeting", "schedule", "overlap"

---

## Pattern Recognition

### Interval Representation

```python
# List of lists
intervals = [[1,3], [2,6], [8,10]]

# List of tuples
intervals = [(1,3), (2,6), (8,10)]

# Named tuple (more readable)
from collections import namedtuple
Interval = namedtuple('Interval', ['start', 'end'])
intervals = [Interval(1,3), Interval(2,6), Interval(8,10)]
```

### Overlap Detection

```python
def overlaps(a, b):
    """Check if two intervals overlap."""
    # a = [start1, end1], b = [start2, end2]
    return a[0] <= b[1] and b[0] <= a[1]

# Example:
# [1,3] and [2,4] overlap (1 <= 4 and 2 <= 3)
# [1,2] and [3,4] don't overlap (1 <= 4 but 3 > 2)
```

### Sorting Strategy

```python
# Sort by start time (most common)
intervals.sort(key=lambda x: x[0])

# Sort by end time (for activity selection)
intervals.sort(key=lambda x: x[1])

# Sort by both (start, then end)
intervals.sort(key=lambda x: (x[0], x[1]))
```

---

## Merge Problems

### Problem 1: Merge Intervals

```python
def merge(intervals: list[list[int]]) -> list[list[int]]:
    """
    Merge overlapping intervals.
    
    Time: O(n log n)
    Space: O(n)
    
    Example: [[1,3],[2,6],[8,10],[15,18]]
    Output: [[1,6],[8,10],[15,18]]
    
    Algorithm:
    1. Sort by start time
    2. Merge overlapping intervals
    """
    if not intervals:
        return []
    
    # Sort by start time
    intervals.sort(key=lambda x: x[0])
    
    merged = [intervals[0]]
    
    for current in intervals[1:]:
        last = merged[-1]
        
        # Check if overlaps
        if current[0] <= last[1]:
            # Merge by updating end time
            last[1] = max(last[1], current[1])
        else:
            # No overlap, add new interval
            merged.append(current)
    
    return merged
```

### Problem 2: Insert Interval

```python
def insert(intervals: list[list[int]], newInterval: list[int]) -> list[list[int]]:
    """
    Insert new interval and merge if necessary.
    
    Time: O(n)
    Space: O(n)
    
    Example: intervals = [[1,3],[6,9]], newInterval = [2,5]
    Output: [[1,5],[6,9]]
    
    Algorithm:
    1. Add all intervals ending before new interval starts
    2. Merge all overlapping intervals with new interval
    3. Add all intervals starting after new interval ends
    """
    result = []
    i = 0
    n = len(intervals)
    
    # Add all intervals before newInterval
    while i < n and intervals[i][1] < newInterval[0]:
        result.append(intervals[i])
        i += 1
    
    # Merge overlapping intervals
    while i < n and intervals[i][0] <= newInterval[1]:
        newInterval[0] = min(newInterval[0], intervals[i][0])
        newInterval[1] = max(newInterval[1], intervals[i][1])
        i += 1
    
    result.append(newInterval)
    
    # Add remaining intervals
    while i < n:
        result.append(intervals[i])
        i += 1
    
    return result
```

### Problem 3: Interval List Intersections

```python
def interval_intersection(
    firstList: list[list[int]], 
    secondList: list[list[int]]
) -> list[list[int]]:
    """
    Find intersection of two interval lists.
    
    Time: O(m + n)
    Space: O(min(m, n))
    
    Example: 
    firstList = [[0,2],[5,10],[13,23],[24,25]]
    secondList = [[1,5],[8,12],[15,24],[25,26]]
    Output: [[1,2],[5,5],[8,10],[15,23],[24,24],[25,25]]
    
    Algorithm: Two pointers, find overlap of current intervals
    """
    result = []
    i = j = 0
    
    while i < len(firstList) and j < len(secondList):
        # Find intersection
        start = max(firstList[i][0], secondList[j][0])
        end = min(firstList[i][1], secondList[j][1])
        
        # Check if valid intersection
        if start <= end:
            result.append([start, end])
        
        # Move pointer of interval that ends first
        if firstList[i][1] < secondList[j][1]:
            i += 1
        else:
            j += 1
    
    return result
```

---

## Scheduling Problems

### Problem 4: Meeting Rooms

```python
def can_attend_meetings(intervals: list[list[int]]) -> bool:
    """
    Check if person can attend all meetings.
    
    Time: O(n log n)
    Space: O(1)
    
    Example: [[0,30],[5,10],[15,20]]
    Output: False (overlap between [0,30] and [5,10])
    
    Algorithm: Sort and check for overlaps
    """
    if not intervals:
        return True
    
    # Sort by start time
    intervals.sort(key=lambda x: x[0])
    
    # Check for overlaps
    for i in range(1, len(intervals)):
        if intervals[i][0] < intervals[i-1][1]:
            return False
    
    return True
```

### Problem 5: Meeting Rooms II (Minimum Rooms)

```python
def min_meeting_rooms(intervals: list[list[int]]) -> int:
    """
    Find minimum meeting rooms needed.
    
    Time: O(n log n)
    Space: O(n)
    
    Example: [[0,30],[5,10],[15,20]]
    Output: 2
    
    Algorithm: Track starts and ends separately
    """
    if not intervals:
        return 0
    
    # Separate starts and ends
    starts = sorted([i[0] for i in intervals])
    ends = sorted([i[1] for i in intervals])
    
    rooms = 0
    max_rooms = 0
    start_ptr = end_ptr = 0
    
    while start_ptr < len(intervals):
        if starts[start_ptr] < ends[end_ptr]:
            # Meeting starts, need room
            rooms += 1
            max_rooms = max(max_rooms, rooms)
            start_ptr += 1
        else:
            # Meeting ends, free room
            rooms -= 1
            end_ptr += 1
    
    return max_rooms

# Alternative: Priority Queue
import heapq

def min_meeting_rooms_pq(intervals: list[list[int]]) -> int:
    """
    Using min heap to track end times.
    
    Time: O(n log n)
    Space: O(n)
    """
    if not intervals:
        return 0
    
    # Sort by start time
    intervals.sort(key=lambda x: x[0])
    
    # Min heap of end times
    heap = []
    
    for start, end in intervals:
        # Remove meetings that ended
        if heap and heap[0] <= start:
            heapq.heappop(heap)
        
        # Add current meeting
        heapq.heappush(heap, end)
    
    return len(heap)
```

### Problem 6: Non-overlapping Intervals

```python
def erase_overlap_intervals(intervals: list[list[int]]) -> int:
    """
    Minimum intervals to remove to make non-overlapping.
    
    Time: O(n log n)
    Space: O(1)
    
    Example: [[1,2],[2,3],[3,4],[1,3]]
    Output: 1 (remove [1,3])
    
    Algorithm: Greedy - keep intervals with earliest end time
    """
    if not intervals:
        return 0
    
    # Sort by end time
    intervals.sort(key=lambda x: x[1])
    
    count = 0
    end = intervals[0][1]
    
    for i in range(1, len(intervals)):
        if intervals[i][0] < end:
            # Overlap, remove current interval
            count += 1
        else:
            # No overlap, update end
            end = intervals[i][1]
    
    return count
```

### Problem 7: Minimum Arrows to Burst Balloons

```python
def find_min_arrow_shots(points: list[list[int]]) -> int:
    """
    Minimum arrows to burst all balloons.
    
    Time: O(n log n)
    Space: O(1)
    
    Example: [[10,16],[2,8],[1,6],[7,12]]
    Output: 2
    
    Algorithm: Similar to non-overlapping intervals
    """
    if not points:
        return 0
    
    # Sort by end position
    points.sort(key=lambda x: x[1])
    
    arrows = 1
    end = points[0][1]
    
    for i in range(1, len(points)):
        if points[i][0] > end:
            # Need new arrow
            arrows += 1
            end = points[i][1]
    
    return arrows
```

---

## Overlap Detection

### Problem 8: Employee Free Time

```python
class Interval:
    def __init__(self, start, end):
        self.start = start
        self.end = end

def employee_free_time(schedule: list[list[Interval]]) -> list[Interval]:
    """
    Find common free time for all employees.
    
    Time: O(n log n)
    Space: O(n)
    
    Example: [[[1,3],[4,6]], [[2,4]], [[2,5],[9,12]]]
    Output: [[6,9]]
    
    Algorithm:
    1. Flatten and sort all intervals
    2. Merge overlapping intervals
    3. Find gaps between merged intervals
    """
    # Flatten all intervals
    intervals = []
    for employee in schedule:
        for interval in employee:
            intervals.append(interval)
    
    # Sort by start time
    intervals.sort(key=lambda x: x.start)
    
    # Find gaps
    result = []
    prev_end = intervals[0].end
    
    for interval in intervals[1:]:
        if interval.start > prev_end:
            # Found gap
            result.append(Interval(prev_end, interval.start))
        
        prev_end = max(prev_end, interval.end)
    
    return result
```

### Problem 9: Data Stream as Disjoint Intervals

```python
class SummaryRanges:
    """
    Maintain disjoint intervals from data stream.
    
    Time: addNum O(n), getIntervals O(1)
    Space: O(n)
    
    Example:
    addNum(1) → [[1,1]]
    addNum(3) → [[1,1],[3,3]]
    addNum(7) → [[1,1],[3,3],[7,7]]
    addNum(2) → [[1,3],[7,7]]
    addNum(6) → [[1,3],[6,7]]
    """
    
    def __init__(self):
        self.intervals = []
    
    def addNum(self, value: int) -> None:
        """Add number and maintain intervals."""
        # Find insertion position
        new_interval = [value, value]
        result = []
        i = 0
        
        # Add intervals before new value
        while i < len(self.intervals) and self.intervals[i][1] < value - 1:
            result.append(self.intervals[i])
            i += 1
        
        # Merge overlapping intervals
        while i < len(self.intervals) and self.intervals[i][0] <= value + 1:
            new_interval[0] = min(new_interval[0], self.intervals[i][0])
            new_interval[1] = max(new_interval[1], self.intervals[i][1])
            i += 1
        
        result.append(new_interval)
        
        # Add remaining intervals
        while i < len(self.intervals):
            result.append(self.intervals[i])
            i += 1
        
        self.intervals = result
    
    def getIntervals(self) -> list[list[int]]:
        """Return current intervals."""
        return self.intervals
```

### Problem 10: Range Module

```python
class RangeModule:
    """
    Track ranges of numbers.
    
    Time: O(n) per operation
    Space: O(n)
    """
    
    def __init__(self):
        self.ranges = []
    
    def addRange(self, left: int, right: int) -> None:
        """Add range [left, right)."""
        result = []
        i = 0
        
        # Add ranges before new range
        while i < len(self.ranges) and self.ranges[i][1] < left:
            result.append(self.ranges[i])
            i += 1
        
        # Merge overlapping ranges
        while i < len(self.ranges) and self.ranges[i][0] <= right:
            left = min(left, self.ranges[i][0])
            right = max(right, self.ranges[i][1])
            i += 1
        
        result.append([left, right])
        
        # Add remaining ranges
        result.extend(self.ranges[i:])
        
        self.ranges = result
    
    def queryRange(self, left: int, right: int) -> bool:
        """Check if [left, right) is tracked."""
        for start, end in self.ranges:
            if start <= left and right <= end:
                return True
        return False
    
    def removeRange(self, left: int, right: int) -> None:
        """Remove range [left, right)."""
        result = []
        
        for start, end in self.ranges:
            if end <= left or start >= right:
                # No overlap
                result.append([start, end])
            else:
                # Overlap, split if needed
                if start < left:
                    result.append([start, left])
                if right < end:
                    result.append([right, end])
        
        self.ranges = result
```

---

## Interview Tips

### 1. Sort First

```python
# Most interval problems start with sorting
intervals.sort(key=lambda x: x[0])  # By start
intervals.sort(key=lambda x: x[1])  # By end
```

### 2. Overlap Detection

```python
def overlaps(a, b):
    """Check if intervals overlap."""
    return a[0] <= b[1] and b[0] <= a[1]

# Example:
# [1,3] and [2,4]: 1<=4 and 2<=3 → True
# [1,2] and [3,4]: 1<=4 but 3>2 → False
```

### 3. Merge Template

```python
def merge_intervals(intervals):
    """Template for merging intervals."""
    if not intervals:
        return []
    
    intervals.sort(key=lambda x: x[0])
    merged = [intervals[0]]
    
    for current in intervals[1:]:
        last = merged[-1]
        
        if current[0] <= last[1]:
            # Overlap, merge
            last[1] = max(last[1], current[1])
        else:
            # No overlap, add new
            merged.append(current)
    
    return merged
```

### 4. Common Patterns

```python
# Pattern 1: Separate Starts and Ends
starts = sorted([i[0] for i in intervals])
ends = sorted([i[1] for i in intervals])

# Pattern 2: Priority Queue for End Times
import heapq
heap = []
for start, end in intervals:
    if heap and heap[0] <= start:
        heapq.heappop(heap)
    heapq.heappush(heap, end)

# Pattern 3: Greedy (Activity Selection)
intervals.sort(key=lambda x: x[1])  # Sort by end
count = 1
end = intervals[0][1]
for i in range(1, len(intervals)):
    if intervals[i][0] >= end:
        count += 1
        end = intervals[i][1]
```

### 5. Edge Cases

```python
# Empty intervals
if not intervals:
    return []

# Single interval
if len(intervals) == 1:
    return intervals

# Check bounds
if start > end:
    return error

# Handle negative numbers
intervals = [[-10, -5], [-3, 0], [2, 5]]
```

---

## Practice Problems

### Easy
1. [Merge Intervals](https://leetcode.com/problems/merge-intervals/)
2. [Meeting Rooms](https://leetcode.com/problems/meeting-rooms/)
3. [Summary Ranges](https://leetcode.com/problems/summary-ranges/)

### Medium
1. [Insert Interval](https://leetcode.com/problems/insert-interval/)
2. [Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/)
3. [Meeting Rooms II](https://leetcode.com/problems/meeting-rooms-ii/)
4. [Minimum Arrows to Burst Balloons](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/)
5. [Interval List Intersections](https://leetcode.com/problems/interval-list-intersections/)
6. [Car Pooling](https://leetcode.com/problems/car-pooling/)
7. [My Calendar I](https://leetcode.com/problems/my-calendar-i/)

### Hard
1. [Employee Free Time](https://leetcode.com/problems/employee-free-time/)
2. [Range Module](https://leetcode.com/problems/range-module/)
3. [Data Stream as Disjoint Intervals](https://leetcode.com/problems/data-stream-as-disjoint-intervals/)

---

## Summary

### Key Takeaways
- ✅ **Sort first** - Usually by start time
- ✅ **Overlap check** - `a[0] <= b[1] and b[0] <= a[1]`
- ✅ **Merge pattern** - Track last interval, merge if overlap
- ✅ **Greedy** - Many problems use greedy approach

### Quick Reference

```python
# Merge Intervals
intervals.sort(key=lambda x: x[0])
merged = [intervals[0]]
for curr in intervals[1:]:
    if curr[0] <= merged[-1][1]:
        merged[-1][1] = max(merged[-1][1], curr[1])
    else:
        merged.append(curr)

# Overlap Check
def overlaps(a, b):
    return a[0] <= b[1] and b[0] <= a[1]

# Minimum Rooms (Sweep Line)
starts = sorted([i[0] for i in intervals])
ends = sorted([i[1] for i in intervals])
rooms = max_rooms = 0
s = e = 0
while s < len(starts):
    if starts[s] < ends[e]:
        rooms += 1
        max_rooms = max(max_rooms, rooms)
        s += 1
    else:
        rooms -= 1
        e += 1
```

---

**Next**: [Math & Geometry →](../11-math-geometry/README.md)

**Happy Coding! 🚀**
