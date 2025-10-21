# 🎯 Two Pointers Pattern

> Master one of the most powerful array/string techniques

---

## 📚 Table of Contents

1. [Pattern Overview](#pattern-overview)
2. [When to Use](#when-to-use)
3. [Common Variations](#common-variations)
4. [Classic Problems](#classic-problems)
5. [Practice Problems](#practice-problems)

---

## Pattern Overview

**Two Pointers** uses two references to traverse data, typically from different positions or directions.

### Key Idea
Instead of nested loops (O(n²)), use two pointers to solve in O(n).

### Visualization
```
Same Direction:
[1, 2, 3, 4, 5, 6]
 ↑slow      ↑fast

Opposite Directions:
[1, 2, 3, 4, 5, 6]
 ↑left         ↑right
```

---

## When to Use

✅ **Use Two Pointers when:**
- Array/string is **sorted**
- Need to find **pairs** with certain property
- Processing from **both ends**
- Need **in-place** modification
- **Partitioning** problem
- Finding elements with **specific relationship**

❌ **Don't use when:**
- Need to check all pairs (still O(n²))
- Random access required
- Data is not sequential

---

## Common Variations

### 1. Opposite Direction (Most Common)

**Start from both ends, move toward center**

```python
def two_pointers_opposite(arr):
    left, right = 0, len(arr) - 1
    
    while left < right:
        # Process arr[left] and arr[right]
        
        # Move pointers based on condition
        if condition:
            left += 1
        else:
            right -= 1
```

**Use Cases:**
- Two Sum (sorted array)
- Valid Palindrome
- Container With Most Water
- Reverse String/Array

### 2. Same Direction (Fast & Slow)

**Both start from beginning, move at different speeds**

```python
def two_pointers_same(arr):
    slow = 0
    fast = 0
    
    while fast < len(arr):
        # Process based on condition
        if condition:
            # Do something with arr[slow]
            slow += 1
        fast += 1
```

**Use Cases:**
- Remove Duplicates
- Move Zeroes
- Remove Element
- Partition Array

### 3. Sliding Window (Special Case)

**Maintain a window between two pointers**

```python
def sliding_window(arr):
    left = 0
    
    for right in range(len(arr)):
        # Add arr[right] to window
        
        while window_invalid:
            # Shrink window from left
            left += 1
```

**Use Cases:**
- Longest Substring
- Minimum Window
- Max Consecutive Ones

---

## Classic Problems

### 1. Two Sum (Sorted Array)

**Problem**: Find two numbers that add up to target.

```python
def two_sum_sorted(numbers: list[int], target: int) -> list[int]:
    """
    Given sorted array, find indices of two numbers that add to target.
    
    Time: O(n)
    Space: O(1)
    
    Pattern: Opposite direction pointers
    """
    left, right = 0, len(numbers) - 1
    
    while left < right:
        current_sum = numbers[left] + numbers[right]
        
        if current_sum == target:
            return [left + 1, right + 1]  # 1-indexed
        elif current_sum < target:
            left += 1  # Need larger sum
        else:
            right -= 1  # Need smaller sum
    
    return []

# Test
print(two_sum_sorted([2, 7, 11, 15], 9))  # [1, 2]
print(two_sum_sorted([2, 3, 4], 6))       # [1, 3]
```

**Key Insight**: Sorted array allows us to decide which pointer to move.

---

### 2. Valid Palindrome

**Problem**: Check if string is palindrome (ignoring non-alphanumeric).

```python
def is_palindrome(s: str) -> bool:
    """
    Check if string is palindrome.
    
    Time: O(n)
    Space: O(1)
    
    Pattern: Opposite direction pointers
    """
    left, right = 0, len(s) - 1
    
    while left < right:
        # Skip non-alphanumeric from left
        while left < right and not s[left].isalnum():
            left += 1
        
        # Skip non-alphanumeric from right
        while left < right and not s[right].isalnum():
            right -= 1
        
        # Compare characters
        if s[left].lower() != s[right].lower():
            return False
        
        left += 1
        right -= 1
    
    return True

# Test
print(is_palindrome("A man, a plan, a canal: Panama"))  # True
print(is_palindrome("race a car"))  # False
```

---

### 3. Container With Most Water

**Problem**: Find two lines that form container with maximum water.

```python
def max_area(height: list[int]) -> int:
    """
    Find maximum area between two vertical lines.
    
    Time: O(n)
    Space: O(1)
    
    Pattern: Opposite direction pointers
    """
    left, right = 0, len(height) - 1
    max_area = 0
    
    while left < right:
        # Calculate current area
        width = right - left
        current_height = min(height[left], height[right])
        current_area = width * current_height
        
        max_area = max(max_area, current_area)
        
        # Move pointer with smaller height
        if height[left] < height[right]:
            left += 1
        else:
            right -= 1
    
    return max_area

# Test
print(max_area([1, 8, 6, 2, 5, 4, 8, 3, 7]))  # 49
```

**Key Insight**: Always move the pointer with smaller height (potential for improvement).

---

### 4. Remove Duplicates from Sorted Array

**Problem**: Remove duplicates in-place, return new length.

```python
def remove_duplicates(nums: list[int]) -> int:
    """
    Remove duplicates from sorted array in-place.
    
    Time: O(n)
    Space: O(1)
    
    Pattern: Same direction (fast & slow)
    """
    if not nums:
        return 0
    
    slow = 0  # Position to write next unique element
    
    for fast in range(1, len(nums)):
        if nums[fast] != nums[slow]:
            slow += 1
            nums[slow] = nums[fast]
    
    return slow + 1

# Test
nums = [1, 1, 2, 2, 3, 4, 4]
length = remove_duplicates(nums)
print(nums[:length])  # [1, 2, 3, 4]
```

**Key Insight**: Slow tracks write position, fast explores array.

---

### 5. Move Zeroes

**Problem**: Move all zeroes to end while maintaining order.

```python
def move_zeroes(nums: list[int]) -> None:
    """
    Move all zeros to end, maintain relative order.
    
    Time: O(n)
    Space: O(1)
    
    Pattern: Same direction (fast & slow)
    """
    slow = 0  # Position for next non-zero
    
    # Move all non-zeros to front
    for fast in range(len(nums)):
        if nums[fast] != 0:
            nums[slow] = nums[fast]
            slow += 1
    
    # Fill remaining with zeros
    while slow < len(nums):
        nums[slow] = 0
        slow += 1

# Test
nums = [0, 1, 0, 3, 12]
move_zeroes(nums)
print(nums)  # [1, 3, 12, 0, 0]
```

**Alternative (Swap-based)**:
```python
def move_zeroes_swap(nums: list[int]) -> None:
    slow = 0
    
    for fast in range(len(nums)):
        if nums[fast] != 0:
            nums[slow], nums[fast] = nums[fast], nums[slow]
            slow += 1

# Test
nums = [0, 1, 0, 3, 12]
move_zeroes_swap(nums)
print(nums)  # [1, 3, 12, 0, 0]
```

---

### 6. 3Sum

**Problem**: Find all unique triplets that sum to zero.

```python
def three_sum(nums: list[int]) -> list[list[int]]:
    """
    Find all unique triplets that sum to zero.
    
    Time: O(n²)
    Space: O(1) excluding output
    
    Pattern: Fix one element, use two pointers for rest
    """
    nums.sort()
    result = []
    
    for i in range(len(nums) - 2):
        # Skip duplicates for first element
        if i > 0 and nums[i] == nums[i - 1]:
            continue
        
        # Two pointers for remaining two numbers
        left, right = i + 1, len(nums) - 1
        target = -nums[i]
        
        while left < right:
            current_sum = nums[left] + nums[right]
            
            if current_sum == target:
                result.append([nums[i], nums[left], nums[right]])
                
                # Skip duplicates
                while left < right and nums[left] == nums[left + 1]:
                    left += 1
                while left < right and nums[right] == nums[right - 1]:
                    right -= 1
                
                left += 1
                right -= 1
            elif current_sum < target:
                left += 1
            else:
                right -= 1
    
    return result

# Test
print(three_sum([-1, 0, 1, 2, -1, -4]))
# [[-1, -1, 2], [-1, 0, 1]]
```

**Key Insight**: Reduce 3Sum to 2Sum by fixing one element.

---

### 7. Trapping Rain Water

**Problem**: Calculate trapped rainwater between bars.

```python
def trap(height: list[int]) -> int:
    """
    Calculate trapped rainwater.
    
    Time: O(n)
    Space: O(1)
    
    Pattern: Opposite direction pointers
    """
    if not height:
        return 0
    
    left, right = 0, len(height) - 1
    left_max, right_max = 0, 0
    water = 0
    
    while left < right:
        if height[left] < height[right]:
            # Process left side
            if height[left] >= left_max:
                left_max = height[left]
            else:
                water += left_max - height[left]
            left += 1
        else:
            # Process right side
            if height[right] >= right_max:
                right_max = height[right]
            else:
                water += right_max - height[right]
            right -= 1
    
    return water

# Test
print(trap([0, 1, 0, 2, 1, 0, 1, 3, 2, 1, 2, 1]))  # 6
```

**Key Insight**: Water level at position determined by min of max heights on both sides.

---

### 8. Sort Colors (Dutch National Flag)

**Problem**: Sort array with 0s, 1s, 2s in-place.

```python
def sort_colors(nums: list[int]) -> None:
    """
    Sort array containing 0, 1, 2 in-place.
    
    Time: O(n)
    Space: O(1)
    
    Pattern: Three pointers (variation)
    """
    low, mid, high = 0, 0, len(nums) - 1
    
    while mid <= high:
        if nums[mid] == 0:
            # Swap with low, move both forward
            nums[low], nums[mid] = nums[mid], nums[low]
            low += 1
            mid += 1
        elif nums[mid] == 1:
            # Already in place, just move mid
            mid += 1
        else:  # nums[mid] == 2
            # Swap with high, move high backward
            nums[mid], nums[high] = nums[high], nums[mid]
            high -= 1
            # Don't move mid (need to check swapped element)

# Test
nums = [2, 0, 2, 1, 1, 0]
sort_colors(nums)
print(nums)  # [0, 0, 1, 1, 2, 2]
```

**Key Insight**: Three regions: [0...low) = 0s, [low...mid) = 1s, (high...n) = 2s.

---

## Problem Templates

### Template 1: Opposite Direction (Sorted Array)

```python
def opposite_direction_template(arr):
    """Find pair in sorted array with some property."""
    left, right = 0, len(arr) - 1
    
    while left < right:
        # Calculate current result
        result = compute(arr[left], arr[right])
        
        if result == target:
            return [left, right]
        elif result < target:
            left += 1  # Need to increase result
        else:
            right -= 1  # Need to decrease result
    
    return []
```

### Template 2: Same Direction (In-place Modification)

```python
def same_direction_template(arr):
    """Remove elements or modify array in-place."""
    slow = 0  # Write position
    
    for fast in range(len(arr)):
        if should_keep(arr[fast]):
            arr[slow] = arr[fast]
            slow += 1
    
    return slow  # New length
```

### Template 3: Sliding Window

```python
def sliding_window_template(arr):
    """Find subarray with specific property."""
    left = 0
    result = 0
    
    for right in range(len(arr)):
        # Add arr[right] to window
        add_to_window(arr[right])
        
        # Shrink window if invalid
        while is_invalid():
            remove_from_window(arr[left])
            left += 1
        
        # Update result with valid window
        result = max(result, right - left + 1)
    
    return result
```

---

## Common Pitfalls

### 1. Off-by-One Errors

```python
# ❌ Wrong: might miss last element
while left < right - 1:
    ...

# ✅ Correct
while left < right:
    ...
```

### 2. Infinite Loops

```python
# ❌ Wrong: pointers might not move
while left < right:
    if condition:
        # Forgot to move pointers!
        pass

# ✅ Correct: ensure pointers always move
while left < right:
    if condition:
        left += 1
    else:
        right -= 1
```

### 3. Not Handling Duplicates

```python
# ❌ Wrong: includes duplicate triplets in 3Sum
result.append([nums[i], nums[left], nums[right]])
left += 1
right -= 1

# ✅ Correct: skip duplicates
result.append([nums[i], nums[left], nums[right]])
while left < right and nums[left] == nums[left + 1]:
    left += 1
while left < right and nums[right] == nums[right - 1]:
    right -= 1
left += 1
right -= 1
```

---

## Interview Tips

### 1. Recognize the Pattern
Look for these keywords:
- "Sorted array"
- "Find pair"
- "Remove/modify in-place"
- "From both ends"

### 2. Choose the Right Variation
- **Opposite**: When processing from both ends
- **Same**: When need in-place modification
- **Window**: When looking for subarray

### 3. Handle Edge Cases
```python
# Empty array
if not arr:
    return []

# Single element
if len(arr) == 1:
    return handle_single()

# Two elements
if len(arr) == 2:
    return handle_two()
```

### 4. Time Complexity
- Opposite/Same direction: **O(n)**
- With sorting first: **O(n log n)**
- Nested (like 3Sum): **O(n²)**

---

## Practice Problems

### Easy
1. [Remove Element](https://leetcode.com/problems/remove-element/)
2. [Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array/)
3. [Reverse String](https://leetcode.com/problems/reverse-string/)
4. [Valid Palindrome](https://leetcode.com/problems/valid-palindrome/)
5. [Squares of Sorted Array](https://leetcode.com/problems/squares-of-a-sorted-array/)

### Medium
1. [3Sum](https://leetcode.com/problems/3sum/)
2. [Container With Most Water](https://leetcode.com/problems/container-with-most-water/)
3. [Sort Colors](https://leetcode.com/problems/sort-colors/)
4. [3Sum Closest](https://leetcode.com/problems/3sum-closest/)
5. [4Sum](https://leetcode.com/problems/4sum/)

### Hard
1. [Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/)
2. [Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/)
3. [Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/)

---

## Summary

### Key Takeaways
- ✅ Two pointers reduce O(n²) to O(n)
- ✅ Three main variations: opposite, same, window
- ✅ Works best with sorted data
- ✅ Essential for in-place operations

### Quick Reference

| Problem Type | Pointer Direction | Time |
|--------------|------------------|------|
| Find pair (sorted) | Opposite | O(n) |
| In-place removal | Same | O(n) |
| Substring/subarray | Window | O(n) |
| k-Sum | Fix + Opposite | O(n^(k-1)) |

---

**Next**: [Sliding Window →](../02-sliding-window/README.md)

**Happy Coding! 🚀**
