# 📊 Monotonic Stack - Python DSA

> Solve next greater/smaller element problems in O(n) time

---

## 📚 Table of Contents

1. [Introduction](#introduction)
2. [Pattern Recognition](#pattern-recognition)
3. [Next Greater Element](#next-greater-element)
4. [Histogram Problems](#histogram-problems)
5. [Temperature and Stock Problems](#temperature-and-stock-problems)
6. [Interview Tips](#interview-tips)
7. [Practice Problems](#practice-problems)

---

## Introduction

**Monotonic Stack** maintains elements in increasing or decreasing order to solve next/previous greater/smaller problems efficiently.

### Key Characteristics
- ✅ **O(n) time** - Each element pushed/popped once
- ✅ **Stack property** - Maintain monotonic order
- ✅ **Next greater/smaller** - Efficient lookups
- ✅ **One pass** - Process array once

### When to Use
- Find **next/previous greater** element
- Find **next/previous smaller** element
- **Histogram** or area problems
- **Temperature**, stock span problems
- Keywords: "next greater", "previous smaller", "histogram", "monotonic"

---

## Pattern Recognition

### Monotonic Stack Types

```python
# Monotonic Increasing Stack (bottom to top)
# Use for: Next smaller element, previous smaller element
stack = []
for num in nums:
    while stack and stack[-1] > num:
        stack.pop()
    stack.append(num)

# Monotonic Decreasing Stack (bottom to top)
# Use for: Next greater element, previous greater element
stack = []
for num in nums:
    while stack and stack[-1] < num:
        stack.pop()
    stack.append(num)
```

### Core Template

```python
def monotonic_stack_template(nums):
    """Template for monotonic stack problems."""
    n = len(nums)
    result = [-1] * n  # or appropriate default
    stack = []  # Store indices
    
    for i in range(n):
        # Maintain monotonic property
        while stack and condition(nums[stack[-1]], nums[i]):
            idx = stack.pop()
            result[idx] = nums[i]  # or i, or calculation
        
        stack.append(i)
    
    return result
```

---

## Next Greater Element

### Problem 1: Next Greater Element I

```python
def next_greater_element(nums1: list[int], nums2: list[int]) -> list[int]:
    """
    Find next greater element for nums1 in nums2.
    
    Time: O(n + m)
    Space: O(n)
    
    Example:
    nums1 = [4,1,2], nums2 = [1,3,4,2]
    Output: [-1,3,-1]
    
    Algorithm: Use monotonic stack + hashmap
    """
    # Build next greater map for nums2
    next_greater = {}
    stack = []
    
    for num in nums2:
        while stack and stack[-1] < num:
            next_greater[stack.pop()] = num
        stack.append(num)
    
    # Map nums1 to next greater
    return [next_greater.get(num, -1) for num in nums1]
```

### Problem 2: Next Greater Element II (Circular)

```python
def next_greater_elements(nums: list[int]) -> list[int]:
    """
    Find next greater element in circular array.
    
    Time: O(n)
    Space: O(n)
    
    Example: nums = [1,2,1]
    Output: [2,-1,2]
    
    Algorithm: Process array twice (simulate circular)
    """
    n = len(nums)
    result = [-1] * n
    stack = []
    
    # Process array twice
    for i in range(2 * n):
        idx = i % n
        
        while stack and nums[stack[-1]] < nums[idx]:
            result[stack.pop()] = nums[idx]
        
        if i < n:
            stack.append(idx)
    
    return result
```

### Problem 3: Daily Temperatures

```python
def daily_temperatures(temperatures: list[int]) -> list[int]:
    """
    Find days until warmer temperature.
    
    Time: O(n)
    Space: O(n)
    
    Example: temperatures = [73,74,75,71,69,72,76,73]
    Output: [1,1,4,2,1,1,0,0]
    
    Algorithm: Monotonic decreasing stack
    """
    n = len(temperatures)
    result = [0] * n
    stack = []  # Store indices
    
    for i in range(n):
        # While current temp is higher
        while stack and temperatures[stack[-1]] < temperatures[i]:
            prev_idx = stack.pop()
            result[prev_idx] = i - prev_idx
        
        stack.append(i)
    
    return result
```

### Problem 4: Online Stock Span

```python
class StockSpanner:
    """
    Calculate stock price span.
    
    Time: O(1) amortized per call
    Space: O(n)
    
    Example:
    StockSpanner() -> None
    next(100) -> 1
    next(80) -> 1
    next(60) -> 1
    next(70) -> 2
    next(60) -> 1
    next(75) -> 4
    next(85) -> 6
    """
    
    def __init__(self):
        self.stack = []  # (price, span)
    
    def next(self, price: int) -> int:
        """Calculate span for current price."""
        span = 1
        
        # Add spans of smaller prices
        while self.stack and self.stack[-1][0] <= price:
            span += self.stack.pop()[1]
        
        self.stack.append((price, span))
        return span
```

---

## Histogram Problems

### Problem 5: Largest Rectangle in Histogram

```python
def largest_rectangle_area(heights: list[int]) -> int:
    """
    Find largest rectangle in histogram.
    
    Time: O(n)
    Space: O(n)
    
    Example: heights = [2,1,5,6,2,3]
    Output: 10 (5×2 rectangle)
    
    Algorithm: Monotonic increasing stack
    """
    stack = []
    max_area = 0
    heights.append(0)  # Sentinel to clear stack
    
    for i, h in enumerate(heights):
        while stack and heights[stack[-1]] > h:
            height = heights[stack.pop()]
            width = i if not stack else i - stack[-1] - 1
            max_area = max(max_area, height * width)
        
        stack.append(i)
    
    heights.pop()  # Remove sentinel
    return max_area
```

### Problem 6: Maximal Rectangle

```python
def maximal_rectangle(matrix: list[list[str]]) -> int:
    """
    Find largest rectangle containing only 1s.
    
    Time: O(m × n)
    Space: O(n)
    
    Example:
    [["1","0","1","0","0"],
     ["1","0","1","1","1"],
     ["1","1","1","1","1"],
     ["1","0","0","1","0"]]
    Output: 6
    
    Algorithm: Use largest rectangle in histogram for each row
    """
    if not matrix or not matrix[0]:
        return 0
    
    n = len(matrix[0])
    heights = [0] * n
    max_area = 0
    
    for row in matrix:
        for i in range(n):
            # Build histogram heights
            heights[i] = heights[i] + 1 if row[i] == '1' else 0
        
        # Find max area in current histogram
        max_area = max(max_area, largest_rectangle_area(heights[:]))
    
    return max_area

def largest_rectangle_area(heights):
    """Helper function from Problem 5."""
    stack = []
    max_area = 0
    heights.append(0)
    
    for i, h in enumerate(heights):
        while stack and heights[stack[-1]] > h:
            height = heights[stack.pop()]
            width = i if not stack else i - stack[-1] - 1
            max_area = max(max_area, height * width)
        stack.append(i)
    
    heights.pop()
    return max_area
```

---

## Temperature and Stock Problems

### Problem 7: Trapping Rain Water

```python
def trap(height: list[int]) -> int:
    """
    Calculate trapped rain water.
    
    Time: O(n)
    Space: O(1)
    
    Example: height = [0,1,0,2,1,0,1,3,2,1,2,1]
    Output: 6
    
    Algorithm: Two pointers (optimal)
    """
    if not height:
        return 0
    
    left, right = 0, len(height) - 1
    left_max = right_max = water = 0
    
    while left < right:
        if height[left] < height[right]:
            if height[left] >= left_max:
                left_max = height[left]
            else:
                water += left_max - height[left]
            left += 1
        else:
            if height[right] >= right_max:
                right_max = height[right]
            else:
                water += right_max - height[right]
            right -= 1
    
    return water

def trap_stack(height: list[int]) -> int:
    """
    Using monotonic stack approach.
    
    Time: O(n)
    Space: O(n)
    """
    stack = []
    water = 0
    
    for i, h in enumerate(height):
        while stack and height[stack[-1]] < h:
            bottom = stack.pop()
            
            if not stack:
                break
            
            width = i - stack[-1] - 1
            bounded_height = min(h, height[stack[-1]]) - height[bottom]
            water += width * bounded_height
        
        stack.append(i)
    
    return water
```

### Problem 8: Remove K Digits

```python
def remove_k_digits(num: str, k: int) -> str:
    """
    Remove k digits to get smallest number.
    
    Time: O(n)
    Space: O(n)
    
    Example: num = "1432219", k = 3
    Output: "1219"
    
    Algorithm: Monotonic increasing stack
    """
    stack = []
    
    for digit in num:
        while stack and k > 0 and stack[-1] > digit:
            stack.pop()
            k -= 1
        stack.append(digit)
    
    # Remove remaining k digits from end
    if k > 0:
        stack = stack[:-k]
    
    # Remove leading zeros
    result = ''.join(stack).lstrip('0')
    
    return result if result else '0'
```

### Problem 9: Sum of Subarray Minimums

```python
def sum_subarray_mins(arr: list[int]) -> int:
    """
    Sum of minimums of all subarrays.
    
    Time: O(n)
    Space: O(n)
    
    Example: arr = [3,1,2,4]
    Output: 17
    Subarrays: [3]=3, [1]=1, [2]=2, [4]=4, [3,1]=1, [1,2]=1, 
               [2,4]=2, [3,1,2]=1, [1,2,4]=1, [3,1,2,4]=1
    Sum = 3+1+2+4+1+1+2+1+1+1 = 17
    
    Algorithm: For each element, find how many subarrays it's minimum of
    """
    MOD = 10**9 + 7
    n = len(arr)
    
    # Find previous less element
    left = [0] * n
    stack = []
    for i in range(n):
        while stack and arr[stack[-1]] > arr[i]:
            stack.pop()
        left[i] = i - stack[-1] if stack else i + 1
        stack.append(i)
    
    # Find next less element
    right = [0] * n
    stack = []
    for i in range(n - 1, -1, -1):
        while stack and arr[stack[-1]] >= arr[i]:
            stack.pop()
        right[i] = stack[-1] - i if stack else n - i
        stack.append(i)
    
    # Calculate sum
    result = 0
    for i in range(n):
        result = (result + arr[i] * left[i] * right[i]) % MOD
    
    return result
```

### Problem 10: 132 Pattern

```python
def find132pattern(nums: list[int]) -> bool:
    """
    Find if there exists i < j < k such that nums[i] < nums[k] < nums[j].
    
    Time: O(n)
    Space: O(n)
    
    Example: nums = [3,1,4,2]
    Output: True (pattern: 1 < 2 < 4)
    
    Algorithm: Monotonic stack from right
    """
    stack = []
    third = float('-inf')
    
    # Traverse from right to left
    for i in range(len(nums) - 1, -1, -1):
        if nums[i] < third:
            return True
        
        # Update third (nums[k])
        while stack and nums[i] > stack[-1]:
            third = stack.pop()
        
        stack.append(nums[i])
    
    return False
```

---

## Interview Tips

### 1. Monotonic Stack Templates

```python
# Template 1: Next Greater Element
def next_greater(nums):
    result = [-1] * len(nums)
    stack = []
    
    for i, num in enumerate(nums):
        while stack and nums[stack[-1]] < num:
            idx = stack.pop()
            result[idx] = num
        stack.append(i)
    
    return result

# Template 2: Previous Smaller Element
def prev_smaller(nums):
    result = [-1] * len(nums)
    stack = []
    
    for i, num in enumerate(nums):
        while stack and nums[stack[-1]] >= num:
            stack.pop()
        if stack:
            result[i] = nums[stack[-1]]
        stack.append(i)
    
    return result

# Template 3: Largest Rectangle
def largest_rectangle(heights):
    stack = []
    max_area = 0
    heights.append(0)  # Sentinel
    
    for i, h in enumerate(heights):
        while stack and heights[stack[-1]] > h:
            height = heights[stack.pop()]
            width = i if not stack else i - stack[-1] - 1
            max_area = max(max_area, height * width)
        stack.append(i)
    
    return max_area
```

### 2. When to Use What

```python
# Monotonic Increasing Stack (elements increase from bottom to top)
# Use for: Next/Previous SMALLER element
for num in nums:
    while stack and stack[-1] > num:
        stack.pop()
    stack.append(num)

# Monotonic Decreasing Stack (elements decrease from bottom to top)
# Use for: Next/Previous GREATER element
for num in nums:
    while stack and stack[-1] < num:
        stack.pop()
    stack.append(num)
```

### 3. Store Index vs Value

```python
# Store indices (more common)
stack = []  # Stores indices
for i in range(len(nums)):
    while stack and nums[stack[-1]] < nums[i]:
        idx = stack.pop()
        # Can access both value and position
    stack.append(i)

# Store values (when index not needed)
stack = []  # Stores values
for num in nums:
    while stack and stack[-1] < num:
        stack.pop()
    stack.append(num)
```

### 4. Common Patterns

```python
# Pattern 1: Circular Array
# Process array twice
for i in range(2 * n):
    idx = i % n
    # ... monotonic stack logic
    if i < n:
        stack.append(idx)

# Pattern 2: With Sentinel
# Add 0 to heights to clear stack
heights.append(0)
# ... process
heights.pop()

# Pattern 3: Count Subarrays
# Find prev/next less/greater for each element
# Multiply distances to count subarrays
count = left_distance * right_distance
```

---

## Practice Problems

### Easy
1. [Next Greater Element I](https://leetcode.com/problems/next-greater-element-i/)
2. [Remove All Adjacent Duplicates](https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string/)

### Medium
1. [Next Greater Element II](https://leetcode.com/problems/next-greater-element-ii/)
2. [Daily Temperatures](https://leetcode.com/problems/daily-temperatures/)
3. [Online Stock Span](https://leetcode.com/problems/online-stock-span/)
4. [Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/)
5. [Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/)
6. [Remove K Digits](https://leetcode.com/problems/remove-k-digits/)
7. [132 Pattern](https://leetcode.com/problems/132-pattern/)
8. [Sum of Subarray Minimums](https://leetcode.com/problems/sum-of-subarray-minimums/)

### Hard
1. [Maximal Rectangle](https://leetcode.com/problems/maximal-rectangle/)
2. [Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/)

---

## Summary

### Key Takeaways
- ✅ **O(n) time** - Each element pushed/popped once
- ✅ **Monotonic property** - Maintain increasing/decreasing order
- ✅ **Next/Previous** - Greater or smaller element
- ✅ **Histogram** - Rectangle area problems

### Quick Reference

```python
# Next Greater Element
result = [-1] * len(nums)
stack = []
for i, num in enumerate(nums):
    while stack and nums[stack[-1]] < num:
        result[stack.pop()] = num
    stack.append(i)

# Largest Rectangle (Histogram)
stack = []
max_area = 0
heights.append(0)
for i, h in enumerate(heights):
    while stack and heights[stack[-1]] > h:
        height = heights[stack.pop()]
        width = i if not stack else i - stack[-1] - 1
        max_area = max(max_area, height * width)
    stack.append(i)

# Stock Span
stack = []  # (price, span)
span = 1
while stack and stack[-1][0] <= price:
    span += stack.pop()[1]
stack.append((price, span))
```

---

**Next**: [Advanced Topics →](../13-advanced-topics/README.md)

**Happy Coding! 🚀**
