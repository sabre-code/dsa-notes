# 👉👈 Two Pointers - Python DSA

> Solve array and string problems efficiently with two pointers

---

## 📚 Table of Contents

1. [Introduction](#introduction)
2. [Pattern Recognition](#pattern-recognition)
3. [Opposite Direction](#opposite-direction)
4. [Same Direction](#same-direction)
5. [Partitioning](#partitioning)
6. [Interview Tips](#interview-tips)
7. [Practice Problems](#practice-problems)

---

## Introduction

**Two Pointers** uses two pointers to traverse data structure, often reducing O(n²) to O(n).

### Key Characteristics
- ✅ **Two pointers** - Start, end, or both from same position
- ✅ **O(n) time** - Single pass through data
- ✅ **Sorted arrays** - Often works best on sorted data
- ✅ **In-place** - Usually O(1) space

### When to Use
- **Sorted arrays** problems
- **Palindrome** checking
- **Pair** finding (sum, product)
- **Partitioning** arrays
- Keywords: "sorted", "pair", "palindrome", "remove duplicates"

---

## Pattern Recognition

### Three Main Patterns

```python
# Pattern 1: Opposite Direction (most common)
left, right = 0, len(arr) - 1
while left < right:
    # Process arr[left] and arr[right]
    left += 1
    right -= 1

# Pattern 2: Same Direction (slow & fast)
slow = fast = 0
while fast < len(arr):
    # Process using both pointers
    fast += 1
    if condition:
        slow += 1

# Pattern 3: Sliding Window Variant
start = 0
for end in range(len(arr)):
    # Expand window
    while condition_violated:
        # Shrink from start
        start += 1
```

---

## Opposite Direction

### Problem 1: Two Sum II (Sorted Array)

```python
def two_sum(numbers: list[int], target: int) -> list[int]:
    """
    Find two numbers that sum to target in sorted array.
    
    Time: O(n)
    Space: O(1)
    
    Example: numbers = [2,7,11,15], target = 9
    Output: [1,2] (1-indexed)
    """
    left, right = 0, len(numbers) - 1
    
    while left < right:
        current_sum = numbers[left] + numbers[right]
        
        if current_sum == target:
            return [left + 1, right + 1]  # 1-indexed
        elif current_sum < target:
            left += 1
        else:
            right -= 1
    
    return []
```

### Problem 2: Valid Palindrome

```python
def is_palindrome(s: str) -> bool:
    """
    Check if string is palindrome (alphanumeric only).
    
    Time: O(n)
    Space: O(1)
    
    Example: s = "A man, a plan, a canal: Panama"
    Output: True
    """
    left, right = 0, len(s) - 1
    
    while left < right:
        # Skip non-alphanumeric
        while left < right and not s[left].isalnum():
            left += 1
        while left < right and not s[right].isalnum():
            right -= 1
        
        # Compare
        if s[left].lower() != s[right].lower():
            return False
        
        left += 1
        right -= 1
    
    return True
```

### Problem 3: Reverse String

```python
def reverse_string(s: list[str]) -> None:
    """
    Reverse string in-place.
    
    Time: O(n)
    Space: O(1)
    
    Example: s = ["h","e","l","l","o"]
    Output: ["o","l","l","e","h"]
    """
    left, right = 0, len(s) - 1
    
    while left < right:
        s[left], s[right] = s[right], s[left]
        left += 1
        right -= 1
```

### Problem 4: Container With Most Water

```python
def max_area(height: list[int]) -> int:
    """
    Find maximum water container can hold.
    
    Time: O(n)
    Space: O(1)
    
    Example: height = [1,8,6,2,5,4,8,3,7]
    Output: 49
    """
    left, right = 0, len(height) - 1
    max_water = 0
    
    while left < right:
        width = right - left
        max_water = max(max_water, width * min(height[left], height[right]))
        
        # Move pointer with smaller height
        if height[left] < height[right]:
            left += 1
        else:
            right -= 1
    
    return max_water
```

### Problem 5: Trapping Rain Water

```python
def trap(height: list[int]) -> int:
    """
    Calculate trapped rain water.
    
    Time: O(n)
    Space: O(1)
    
    Example: height = [0,1,0,2,1,0,1,3,2,1,2,1]
    Output: 6
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
```

---

## Same Direction

### Problem 6: Remove Duplicates from Sorted Array

```python
def remove_duplicates(nums: list[int]) -> int:
    """
    Remove duplicates in-place, return new length.
    
    Time: O(n)
    Space: O(1)
    
    Example: nums = [1,1,2]
    Output: 2, nums = [1,2,_]
    """
    if not nums:
        return 0
    
    slow = 0
    
    for fast in range(1, len(nums)):
        if nums[fast] != nums[slow]:
            slow += 1
            nums[slow] = nums[fast]
    
    return slow + 1
```

### Problem 7: Remove Element

```python
def remove_element(nums: list[int], val: int) -> int:
    """
    Remove all occurrences of val in-place.
    
    Time: O(n)
    Space: O(1)
    
    Example: nums = [3,2,2,3], val = 3
    Output: 2, nums = [2,2,_,_]
    """
    slow = 0
    
    for fast in range(len(nums)):
        if nums[fast] != val:
            nums[slow] = nums[fast]
            slow += 1
    
    return slow
```

### Problem 8: Move Zeroes

```python
def move_zeroes(nums: list[int]) -> None:
    """
    Move all zeros to end, maintain relative order.
    
    Time: O(n)
    Space: O(1)
    
    Example: nums = [0,1,0,3,12]
    Output: [1,3,12,0,0]
    """
    slow = 0
    
    # Move non-zero elements forward
    for fast in range(len(nums)):
        if nums[fast] != 0:
            nums[slow], nums[fast] = nums[fast], nums[slow]
            slow += 1
```

### Problem 9: Sort Colors (Dutch National Flag)

```python
def sort_colors(nums: list[int]) -> None:
    """
    Sort array with values 0, 1, 2 in-place.
    
    Time: O(n)
    Space: O(1)
    
    Example: nums = [2,0,2,1,1,0]
    Output: [0,0,1,1,2,2]
    
    Algorithm: Three-way partitioning
    """
    low = mid = 0
    high = len(nums) - 1
    
    while mid <= high:
        if nums[mid] == 0:
            nums[low], nums[mid] = nums[mid], nums[low]
            low += 1
            mid += 1
        elif nums[mid] == 1:
            mid += 1
        else:  # nums[mid] == 2
            nums[mid], nums[high] = nums[high], nums[mid]
            high -= 1
```

### Problem 10: Squares of Sorted Array

```python
def sorted_squares(nums: list[int]) -> list[int]:
    """
    Return squares of sorted array in sorted order.
    
    Time: O(n)
    Space: O(n)
    
    Example: nums = [-4,-1,0,3,10]
    Output: [0,1,9,16,100]
    """
    n = len(nums)
    result = [0] * n
    left, right = 0, n - 1
    pos = n - 1
    
    while left <= right:
        left_sq = nums[left] * nums[left]
        right_sq = nums[right] * nums[right]
        
        if left_sq > right_sq:
            result[pos] = left_sq
            left += 1
        else:
            result[pos] = right_sq
            right -= 1
        
        pos -= 1
    
    return result
```

---

## Partitioning

### Problem 11: Partition Labels

```python
def partition_labels(s: str) -> list[int]:
    """
    Partition string into maximum parts where each letter appears in at most one part.
    
    Time: O(n)
    Space: O(1)
    
    Example: s = "ababcbacadefegdehijhklij"
    Output: [9,7,8]
    """
    # Find last occurrence of each character
    last = {char: i for i, char in enumerate(s)}
    
    result = []
    start = end = 0
    
    for i, char in enumerate(s):
        end = max(end, last[char])
        
        if i == end:
            result.append(end - start + 1)
            start = i + 1
    
    return result
```

### Problem 12: 3Sum

```python
def three_sum(nums: list[int]) -> list[list[int]]:
    """
    Find all unique triplets that sum to zero.
    
    Time: O(n²)
    Space: O(1)
    
    Example: nums = [-1,0,1,2,-1,-4]
    Output: [[-1,-1,2],[-1,0,1]]
    """
    nums.sort()
    result = []
    
    for i in range(len(nums) - 2):
        # Skip duplicates
        if i > 0 and nums[i] == nums[i-1]:
            continue
        
        left, right = i + 1, len(nums) - 1
        
        while left < right:
            total = nums[i] + nums[left] + nums[right]
            
            if total < 0:
                left += 1
            elif total > 0:
                right -= 1
            else:
                result.append([nums[i], nums[left], nums[right]])
                
                # Skip duplicates
                while left < right and nums[left] == nums[left+1]:
                    left += 1
                while left < right and nums[right] == nums[right-1]:
                    right -= 1
                
                left += 1
                right -= 1
    
    return result
```

### Problem 13: 4Sum

```python
def four_sum(nums: list[int], target: int) -> list[list[int]]:
    """
    Find all unique quadruplets that sum to target.
    
    Time: O(n³)
    Space: O(1)
    
    Example: nums = [1,0,-1,0,-2,2], target = 0
    Output: [[-2,-1,1,2],[-2,0,0,2],[-1,0,0,1]]
    """
    nums.sort()
    result = []
    n = len(nums)
    
    for i in range(n - 3):
        if i > 0 and nums[i] == nums[i-1]:
            continue
        
        for j in range(i + 1, n - 2):
            if j > i + 1 and nums[j] == nums[j-1]:
                continue
            
            left, right = j + 1, n - 1
            
            while left < right:
                total = nums[i] + nums[j] + nums[left] + nums[right]
                
                if total < target:
                    left += 1
                elif total > target:
                    right -= 1
                else:
                    result.append([nums[i], nums[j], nums[left], nums[right]])
                    
                    while left < right and nums[left] == nums[left+1]:
                        left += 1
                    while left < right and nums[right] == nums[right-1]:
                        right -= 1
                    
                    left += 1
                    right -= 1
    
    return result
```

---

## Interview Tips

### 1. Two Pointers Templates

```python
# Template 1: Opposite Direction
def opposite_direction(arr):
    left, right = 0, len(arr) - 1
    
    while left < right:
        # Process arr[left] and arr[right]
        if condition:
            # Found answer
            return result
        elif arr[left] + arr[right] < target:
            left += 1
        else:
            right -= 1

# Template 2: Same Direction (Slow & Fast)
def same_direction(arr):
    slow = 0
    
    for fast in range(len(arr)):
        if arr[fast] meets_condition:
            arr[slow] = arr[fast]
            slow += 1
    
    return slow

# Template 3: Three Pointers (Dutch Flag)
def three_pointers(arr):
    low = mid = 0
    high = len(arr) - 1
    
    while mid <= high:
        if arr[mid] == 0:
            arr[low], arr[mid] = arr[mid], arr[low]
            low += 1
            mid += 1
        elif arr[mid] == 1:
            mid += 1
        else:
            arr[mid], arr[high] = arr[high], arr[mid]
            high -= 1
```

### 2. Common Patterns

```python
# Pattern 1: Two Sum (Sorted)
left, right = 0, len(arr) - 1
while left < right:
    if arr[left] + arr[right] == target:
        return [left, right]
    elif arr[left] + arr[right] < target:
        left += 1
    else:
        right -= 1

# Pattern 2: Remove Duplicates
slow = 0
for fast in range(1, len(arr)):
    if arr[fast] != arr[slow]:
        slow += 1
        arr[slow] = arr[fast]

# Pattern 3: Partition
low, high = 0, len(arr) - 1
while condition:
    if arr[i] < pivot:
        swap with low, increment low
    elif arr[i] > pivot:
        swap with high, decrement high
```

### 3. When to Use

```python
# Use Two Pointers when:
# 1. Array/string is sorted
# 2. Need to find pair with certain sum/product
# 3. Partitioning problems
# 4. In-place operations
# 5. Can reduce from O(n²) to O(n)

# Don't use when:
# - Need to preserve original order (unless allowed to sort)
# - Random access required
# - Need to track multiple states
```

---

## Practice Problems

### Easy
1. [Two Sum II](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)
2. [Valid Palindrome](https://leetcode.com/problems/valid-palindrome/)
3. [Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)
4. [Remove Element](https://leetcode.com/problems/remove-element/)
5. [Move Zeroes](https://leetcode.com/problems/move-zeroes/)
6. [Reverse String](https://leetcode.com/problems/reverse-string/)
7. [Squares of Sorted Array](https://leetcode.com/problems/squares-of-a-sorted-array/)

### Medium
1. [3Sum](https://leetcode.com/problems/3sum/)
2. [4Sum](https://leetcode.com/problems/4sum/)
3. [Container With Most Water](https://leetcode.com/problems/container-with-most-water/)
4. [Sort Colors](https://leetcode.com/problems/sort-colors/)
5. [Partition Labels](https://leetcode.com/problems/partition-labels/)
6. [Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/)

### Hard
1. [Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/)
2. [Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/)

---

## Summary

### Key Takeaways
- ✅ **Two pointers** - Opposite or same direction
- ✅ **O(n) time** - Single pass optimization
- ✅ **Sorted arrays** - Works best on sorted data
- ✅ **In-place** - O(1) space complexity

### Quick Reference

```python
# Opposite Direction
left, right = 0, len(arr) - 1
while left < right:
    if condition_met:
        return result
    elif need_larger:
        left += 1
    else:
        right -= 1

# Same Direction
slow = 0
for fast in range(len(arr)):
    if arr[fast] is_valid:
        arr[slow] = arr[fast]
        slow += 1

# Three Pointers (Dutch Flag)
low = mid = 0
high = len(arr) - 1
while mid <= high:
    # Partition into 3 sections
    pass
```

---

**Next**: [Sorting Algorithms →](../14-sorting/README.md)

**Happy Coding! 🚀**
