# 🔍 Binary Search - Python DSA

> Efficiently search sorted arrays in O(log n)

---

## 📚 Table of Contents

1. [Introduction](#introduction)
2. [Basic Binary Search](#basic-binary-search)
3. [Search Variants](#search-variants)
4. [Search Space Reduction](#search-space-reduction)
5. [Classic Problems](#classic-problems)
6. [Interview Tips](#interview-tips)
7. [Practice Problems](#practice-problems)

---

## Introduction

**Binary Search** repeatedly divides search space in half to find target efficiently.

### Key Characteristics
- ✅ **O(log n) time** - Much faster than linear O(n)
- ✅ **Requires sorted data** - Or monotonic property
- ✅ **Divide and conquer** - Eliminate half each iteration
- ✅ **Multiple variants** - Finding boundaries, rotated arrays

### When to Use
- Array is **sorted** or has monotonic property
- Need to find **specific element** or position
- Can **eliminate half** the search space each step
- Keywords: "sorted array", "find target", "search"

### Real-World Examples
- 📖 Dictionary lookup
- 📚 Library catalog search
- 🎯 Guessing game (high/low)
- 📊 Database indexing

---

## Basic Binary Search

### Template (Iterative)

```python
def binary_search(arr: list[int], target: int) -> int:
    """
    Find index of target in sorted array.
    
    Time: O(log n)
    Space: O(1)
    
    Returns: Index if found, -1 otherwise
    """
    left, right = 0, len(arr) - 1
    
    while left <= right:
        mid = left + (right - left) // 2  # Avoid overflow
        
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            left = mid + 1  # Search right half
        else:
            right = mid - 1  # Search left half
    
    return -1  # Not found

# Test
arr = [1, 3, 5, 7, 9, 11, 13]
print(binary_search(arr, 7))   # 3
print(binary_search(arr, 6))   # -1
```

### Template (Recursive)

```python
def binary_search_recursive(arr: list[int], target: int, left: int = 0, right: int = None) -> int:
    """
    Recursive binary search.
    
    Time: O(log n)
    Space: O(log n) due to recursion stack
    """
    if right is None:
        right = len(arr) - 1
    
    if left > right:
        return -1
    
    mid = left + (right - left) // 2
    
    if arr[mid] == target:
        return mid
    elif arr[mid] < target:
        return binary_search_recursive(arr, target, mid + 1, right)
    else:
        return binary_search_recursive(arr, target, left, mid - 1)

# Test
print(binary_search_recursive([1, 3, 5, 7, 9], 5))  # 2
```

### Using Python's bisect Module

```python
import bisect

arr = [1, 3, 5, 7, 9, 11]

# bisect_left: leftmost insertion position
print(bisect.bisect_left(arr, 5))   # 2 (index of 5)
print(bisect.bisect_left(arr, 6))   # 3 (where 6 would go)

# bisect_right: rightmost insertion position
print(bisect.bisect_right(arr, 5))  # 3 (after 5)
print(bisect.bisect_right(arr, 6))  # 3 (where 6 would go)

# bisect (alias for bisect_right)
print(bisect.bisect(arr, 5))        # 3

# insort: insert and keep sorted
bisect.insort(arr, 6)
print(arr)  # [1, 3, 5, 6, 7, 9, 11]
```

---

## Search Variants

### 1. Find First Occurrence

```python
def find_first(arr: list[int], target: int) -> int:
    """
    Find leftmost occurrence of target.
    
    Time: O(log n)
    Space: O(1)
    
    Example: arr = [1,2,2,2,3], target = 2
    Output: 1 (first index of 2)
    """
    left, right = 0, len(arr) - 1
    result = -1
    
    while left <= right:
        mid = left + (right - left) // 2
        
        if arr[mid] == target:
            result = mid
            right = mid - 1  # Continue searching left
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    
    return result

# Test
print(find_first([1,2,2,2,3], 2))  # 1
```

### 2. Find Last Occurrence

```python
def find_last(arr: list[int], target: int) -> int:
    """
    Find rightmost occurrence of target.
    
    Time: O(log n)
    Space: O(1)
    
    Example: arr = [1,2,2,2,3], target = 2
    Output: 3 (last index of 2)
    """
    left, right = 0, len(arr) - 1
    result = -1
    
    while left <= right:
        mid = left + (right - left) // 2
        
        if arr[mid] == target:
            result = mid
            left = mid + 1  # Continue searching right
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    
    return result

# Test
print(find_last([1,2,2,2,3], 2))  # 3
```

### 3. Find First and Last Position

```python
def search_range(nums: list[int], target: int) -> list[int]:
    """
    Find start and end positions of target.
    
    Time: O(log n)
    Space: O(1)
    
    Example: nums = [5,7,7,8,8,10], target = 8
    Output: [3,4]
    """
    def find_boundary(is_left):
        left, right = 0, len(nums) - 1
        result = -1
        
        while left <= right:
            mid = left + (right - left) // 2
            
            if nums[mid] == target:
                result = mid
                if is_left:
                    right = mid - 1
                else:
                    left = mid + 1
            elif nums[mid] < target:
                left = mid + 1
            else:
                right = mid - 1
        
        return result
    
    return [find_boundary(True), find_boundary(False)]

# Test
print(search_range([5,7,7,8,8,10], 8))  # [3, 4]
```

### 4. Search Insert Position

```python
def search_insert(nums: list[int], target: int) -> int:
    """
    Find index where target should be inserted.
    
    Time: O(log n)
    Space: O(1)
    
    Example: nums = [1,3,5,6], target = 5
    Output: 2
    """
    left, right = 0, len(nums) - 1
    
    while left <= right:
        mid = left + (right - left) // 2
        
        if nums[mid] == target:
            return mid
        elif nums[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    
    return left  # Insertion position

# Test
print(search_insert([1,3,5,6], 5))  # 2
print(search_insert([1,3,5,6], 2))  # 1
print(search_insert([1,3,5,6], 7))  # 4
```

---

## Search Space Reduction

Binary search on answer space (not just arrays).

### Problem 1: Find Peak Element

```python
def find_peak_element(nums: list[int]) -> int:
    """
    Find any peak element index.
    
    Time: O(log n)
    Space: O(1)
    
    Example: nums = [1,2,3,1]
    Output: 2 (element 3 is peak)
    """
    left, right = 0, len(nums) - 1
    
    while left < right:
        mid = left + (right - left) // 2
        
        if nums[mid] > nums[mid + 1]:
            # Peak is on left (including mid)
            right = mid
        else:
            # Peak is on right
            left = mid + 1
    
    return left

# Test
print(find_peak_element([1,2,3,1]))      # 2
print(find_peak_element([1,2,1,3,5,6,4]))  # 5
```

### Problem 2: Search in Rotated Sorted Array

```python
def search_rotated(nums: list[int], target: int) -> int:
    """
    Search in rotated sorted array.
    
    Time: O(log n)
    Space: O(1)
    
    Example: nums = [4,5,6,7,0,1,2], target = 0
    Output: 4
    """
    left, right = 0, len(nums) - 1
    
    while left <= right:
        mid = left + (right - left) // 2
        
        if nums[mid] == target:
            return mid
        
        # Determine which half is sorted
        if nums[left] <= nums[mid]:
            # Left half is sorted
            if nums[left] <= target < nums[mid]:
                right = mid - 1
            else:
                left = mid + 1
        else:
            # Right half is sorted
            if nums[mid] < target <= nums[right]:
                left = mid + 1
            else:
                right = mid - 1
    
    return -1

# Test
print(search_rotated([4,5,6,7,0,1,2], 0))  # 4
print(search_rotated([4,5,6,7,0,1,2], 3))  # -1
```

### Problem 3: Find Minimum in Rotated Sorted Array

```python
def find_min_rotated(nums: list[int]) -> int:
    """
    Find minimum in rotated sorted array.
    
    Time: O(log n)
    Space: O(1)
    
    Example: nums = [3,4,5,1,2]
    Output: 1
    """
    left, right = 0, len(nums) - 1
    
    while left < right:
        mid = left + (right - left) // 2
        
        if nums[mid] > nums[right]:
            # Minimum is in right half
            left = mid + 1
        else:
            # Minimum is in left half (including mid)
            right = mid
    
    return nums[left]

# Test
print(find_min_rotated([3,4,5,1,2]))  # 1
print(find_min_rotated([4,5,6,7,0,1,2]))  # 0
```

### Problem 4: Koko Eating Bananas

```python
import math

def min_eating_speed(piles: list[int], h: int) -> int:
    """
    Minimum eating speed to finish in h hours.
    
    Time: O(n log m) where m is max(piles)
    Space: O(1)
    
    Example: piles = [3,6,7,11], h = 8
    Output: 4
    """
    def can_finish(speed):
        hours = 0
        for pile in piles:
            hours += math.ceil(pile / speed)
        return hours <= h
    
    left, right = 1, max(piles)
    
    while left < right:
        mid = left + (right - left) // 2
        
        if can_finish(mid):
            right = mid  # Try slower speed
        else:
            left = mid + 1  # Need faster speed
    
    return left

# Test
print(min_eating_speed([3,6,7,11], 8))  # 4
```

### Problem 5: Capacity to Ship Packages

```python
def ship_within_days(weights: list[int], days: int) -> int:
    """
    Minimum capacity to ship within days.
    
    Time: O(n log(sum - max))
    Space: O(1)
    
    Example: weights = [1,2,3,4,5,6,7,8,9,10], days = 5
    Output: 15
    """
    def can_ship(capacity):
        current_load = 0
        days_needed = 1
        
        for weight in weights:
            if current_load + weight > capacity:
                days_needed += 1
                current_load = weight
            else:
                current_load += weight
        
        return days_needed <= days
    
    left = max(weights)  # Minimum capacity
    right = sum(weights)  # Maximum capacity
    
    while left < right:
        mid = left + (right - left) // 2
        
        if can_ship(mid):
            right = mid
        else:
            left = mid + 1
    
    return left

# Test
print(ship_within_days([1,2,3,4,5,6,7,8,9,10], 5))  # 15
```

---

## Classic Problems

### Problem 6: Sqrt(x)

```python
def my_sqrt(x: int) -> int:
    """
    Integer square root.
    
    Time: O(log x)
    Space: O(1)
    
    Example: x = 8
    Output: 2 (sqrt(8) = 2.828...)
    """
    if x < 2:
        return x
    
    left, right = 1, x // 2
    
    while left <= right:
        mid = left + (right - left) // 2
        square = mid * mid
        
        if square == x:
            return mid
        elif square < x:
            left = mid + 1
        else:
            right = mid - 1
    
    return right  # Floor value

# Test
print(my_sqrt(4))   # 2
print(my_sqrt(8))   # 2
print(my_sqrt(16))  # 4
```

### Problem 7: Valid Perfect Square

```python
def is_perfect_square(num: int) -> bool:
    """
    Check if number is perfect square.
    
    Time: O(log n)
    Space: O(1)
    """
    if num < 2:
        return True
    
    left, right = 2, num // 2
    
    while left <= right:
        mid = left + (right - left) // 2
        square = mid * mid
        
        if square == num:
            return True
        elif square < num:
            left = mid + 1
        else:
            right = mid - 1
    
    return False

# Test
print(is_perfect_square(16))  # True
print(is_perfect_square(14))  # False
```

### Problem 8: Find Smallest Letter Greater Than Target

```python
def next_greatest_letter(letters: list[str], target: str) -> str:
    """
    Find smallest letter greater than target.
    
    Time: O(log n)
    Space: O(1)
    
    Example: letters = ['c','f','j'], target = 'a'
    Output: 'c'
    """
    left, right = 0, len(letters) - 1
    
    # If target >= last letter, wrap around
    if target >= letters[-1]:
        return letters[0]
    
    while left < right:
        mid = left + (right - left) // 2
        
        if letters[mid] <= target:
            left = mid + 1
        else:
            right = mid
    
    return letters[left]

# Test
print(next_greatest_letter(['c','f','j'], 'a'))  # 'c'
print(next_greatest_letter(['c','f','j'], 'c'))  # 'f'
```

### Problem 9: Single Element in Sorted Array

```python
def single_non_duplicate(nums: list[int]) -> int:
    """
    Find element that appears once in sorted array.
    
    Time: O(log n)
    Space: O(1)
    
    Example: nums = [1,1,2,3,3,4,4,8,8]
    Output: 2
    """
    left, right = 0, len(nums) - 1
    
    while left < right:
        mid = left + (right - left) // 2
        
        # Ensure mid is even
        if mid % 2 == 1:
            mid -= 1
        
        if nums[mid] == nums[mid + 1]:
            # Pair is intact, single is on right
            left = mid + 2
        else:
            # Pair is broken, single is on left
            right = mid
    
    return nums[left]

# Test
print(single_non_duplicate([1,1,2,3,3,4,4,8,8]))  # 2
```

### Problem 10: Search 2D Matrix

```python
def search_matrix(matrix: list[list[int]], target: int) -> bool:
    """
    Search in sorted 2D matrix.
    
    Time: O(log(m*n))
    Space: O(1)
    
    Example: matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 3
    Output: True
    """
    if not matrix or not matrix[0]:
        return False
    
    m, n = len(matrix), len(matrix[0])
    left, right = 0, m * n - 1
    
    while left <= right:
        mid = left + (right - left) // 2
        row = mid // n
        col = mid % n
        mid_value = matrix[row][col]
        
        if mid_value == target:
            return True
        elif mid_value < target:
            left = mid + 1
        else:
            right = mid - 1
    
    return False

# Test
matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]]
print(search_matrix(matrix, 3))   # True
print(search_matrix(matrix, 13))  # False
```

### Problem 11: Search 2D Matrix II

```python
def search_matrix_ii(matrix: list[list[int]], target: int) -> bool:
    """
    Search in row and column sorted matrix.
    
    Time: O(m + n)
    Space: O(1)
    
    Example: matrix = [[1,4,7,11,15],[2,5,8,12,19]], target = 5
    Output: True
    """
    if not matrix or not matrix[0]:
        return False
    
    rows, cols = len(matrix), len(matrix[0])
    row, col = 0, cols - 1  # Start top-right
    
    while row < rows and col >= 0:
        if matrix[row][col] == target:
            return True
        elif matrix[row][col] < target:
            row += 1  # Move down
        else:
            col -= 1  # Move left
    
    return False

# Test
matrix = [[1,4,7,11,15],[2,5,8,12,19],[3,6,9,16,22]]
print(search_matrix_ii(matrix, 5))   # True
print(search_matrix_ii(matrix, 20))  # False
```

### Problem 12: Time-Based Key-Value Store

```python
class TimeMap:
    """
    Store and retrieve values by timestamp.
    
    Time: set O(1), get O(log n)
    Space: O(n)
    """
    def __init__(self):
        from collections import defaultdict
        self.store = defaultdict(list)
    
    def set(self, key: str, value: str, timestamp: int) -> None:
        self.store[key].append((timestamp, value))
    
    def get(self, key: str, timestamp: int) -> str:
        if key not in self.store:
            return ""
        
        values = self.store[key]
        left, right = 0, len(values) - 1
        result = ""
        
        while left <= right:
            mid = left + (right - left) // 2
            
            if values[mid][0] <= timestamp:
                result = values[mid][1]
                left = mid + 1
            else:
                right = mid - 1
        
        return result

# Test
tm = TimeMap()
tm.set("foo", "bar", 1)
print(tm.get("foo", 1))   # "bar"
print(tm.get("foo", 3))   # "bar"
tm.set("foo", "bar2", 4)
print(tm.get("foo", 4))   # "bar2"
print(tm.get("foo", 5))   # "bar2"
```

---

## Interview Tips

### 1. Binary Search Template Choice

```python
# Template 1: Most common (left <= right)
while left <= right:
    mid = left + (right - left) // 2
    if arr[mid] == target:
        return mid
    elif arr[mid] < target:
        left = mid + 1
    else:
        right = mid - 1
return -1

# Template 2: Finding boundary (left < right)
while left < right:
    mid = left + (right - left) // 2
    if condition(mid):
        right = mid
    else:
        left = mid + 1
return left

# Template 3: Three-way split (left + 1 < right)
while left + 1 < right:
    mid = left + (right - left) // 2
    if arr[mid] == target:
        return mid
    elif arr[mid] < target:
        left = mid
    else:
        right = mid
# Check left and right
```

### 2. Common Pitfalls

```python
# ❌ Overflow (in other languages)
mid = (left + right) // 2

# ✅ Safe calculation
mid = left + (right - left) // 2

# ❌ Infinite loop
while left < right:
    mid = left + (right - left) // 2
    left = mid  # Can cause infinite loop!

# ✅ Proper update
while left < right:
    mid = left + (right - left) // 2
    if condition:
        right = mid
    else:
        left = mid + 1  # Always move forward
```

### 3. When NOT to Use Binary Search

```python
# ❌ Unsorted data
arr = [5, 2, 8, 1, 9]

# ❌ Linked list (no random access)
# Use two pointer or other techniques

# ❌ When O(n) is required anyway
# E.g., need to check every element
```

### 4. Search Space Reduction Pattern

```python
# Binary search on answer space
def min_value_satisfying_condition(arr):
    left, right = min_possible, max_possible
    
    while left < right:
        mid = left + (right - left) // 2
        
        if is_valid(mid):
            right = mid  # Try smaller
        else:
            left = mid + 1  # Need larger
    
    return left
```

---

## Practice Problems

### Easy
1. [Binary Search](https://leetcode.com/problems/binary-search/)
2. [Search Insert Position](https://leetcode.com/problems/search-insert-position/)
3. [Sqrt(x)](https://leetcode.com/problems/sqrtx/)
4. [Valid Perfect Square](https://leetcode.com/problems/valid-perfect-square/)
5. [First Bad Version](https://leetcode.com/problems/first-bad-version/)

### Medium
1. [Find First and Last Position](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/)
2. [Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/)
3. [Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/)
4. [Find Peak Element](https://leetcode.com/problems/find-peak-element/)
5. [Search a 2D Matrix](https://leetcode.com/problems/search-a-2d-matrix/)
6. [Koko Eating Bananas](https://leetcode.com/problems/koko-eating-bananas/)
7. [Capacity To Ship Packages](https://leetcode.com/problems/capacity-to-ship-packages-within-d-days/)
8. [Time Based Key-Value Store](https://leetcode.com/problems/time-based-key-value-store/)

### Hard
1. [Median of Two Sorted Arrays](https://leetcode.com/problems/median-of-two-sorted-arrays/)
2. [Find Minimum in Rotated Sorted Array II](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array-ii/)
3. [Split Array Largest Sum](https://leetcode.com/problems/split-array-largest-sum/)

---

## Summary

### Key Takeaways
- ✅ **O(log n) efficiency** - Much faster than linear
- ✅ **Requires sorted/monotonic** - Or search space
- ✅ **Multiple templates** - Choose based on problem
- ✅ **Beyond arrays** - Search on answer space

### Quick Reference

```python
# Basic Binary Search
left, right = 0, len(arr) - 1
while left <= right:
    mid = left + (right - left) // 2
    if arr[mid] == target:
        return mid
    elif arr[mid] < target:
        left = mid + 1
    else:
        right = mid - 1

# Find Boundary
left, right = 0, len(arr) - 1
while left < right:
    mid = left + (right - left) // 2
    if condition(mid):
        right = mid
    else:
        left = mid + 1
return left

# Python bisect
import bisect
bisect.bisect_left(arr, target)   # Insert position (left)
bisect.bisect_right(arr, target)  # Insert position (right)
```

---

**Next**: [Dynamic Programming →](../03-dynamic-programming/README.md)

**Happy Coding! 🚀**
