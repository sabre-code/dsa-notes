# 🪟 Sliding Window - Python DSA

> Optimize array/string problems with dynamic windows

---

## 📚 Table of Contents

1. [Introduction](#introduction)
2. [Pattern Recognition](#pattern-recognition)
3. [Fixed Window](#fixed-window)
4. [Variable Window](#variable-window)
5. [Classic Problems](#classic-problems)
6. [Interview Tips](#interview-tips)
7. [Practice Problems](#practice-problems)

---

## Introduction

**Sliding Window** is an optimization technique for problems involving subarrays/substrings.

### Key Characteristics
- ✅ **Optimize from O(n²) to O(n)** - Avoid nested loops
- ✅ **Two types** - Fixed size and variable size windows
- ✅ **Maintain state** - Track window properties efficiently
- ✅ **Two pointers** - Left and right boundaries

### When to Use
- Problem asks about **contiguous** subarray/substring
- Need to find **min/max/longest/shortest** length
- Keywords: "subarray", "substring", "consecutive elements"
- Optimize brute force O(n²) solutions

### Real-World Examples
- 📊 Moving averages in time series
- 🔍 Pattern matching in text
- 🌡️ Temperature monitoring (last N readings)
- 📈 Stock price analysis (sliding periods)

---

## Pattern Recognition

### Sliding Window Indicators

```python
# Look for these patterns:

# 1. Find longest/shortest subarray with condition
"Find the longest substring with at most K distinct characters"

# 2. Find subarray with exact sum/product
"Find subarray with sum equal to K"

# 3. Find all subarrays satisfying condition
"Find all anagrams in a string"

# 4. Maximum/minimum in every window
"Maximum in every window of size K"

# 5. Contiguous elements
"Find consecutive elements that sum to target"
```

---

## Fixed Window

Window size remains constant throughout.

### Template

```python
def fixed_window(arr, k):
    """
    Fixed window template.
    
    Time: O(n)
    Space: O(1)
    """
    n = len(arr)
    if n < k:
        return []
    
    # Initialize window
    window_sum = sum(arr[:k])
    result = [window_sum]
    
    # Slide window
    for i in range(k, n):
        # Remove leftmost element
        window_sum -= arr[i - k]
        # Add new element
        window_sum += arr[i]
        result.append(window_sum)
    
    return result
```

### Problem 1: Maximum Average Subarray

```python
def find_max_average(nums: list[int], k: int) -> float:
    """
    Find max average of subarray of length k.
    
    Time: O(n)
    Space: O(1)
    
    Example: nums = [1,12,-5,-6,50,3], k = 4
    Output: 12.75 (subarray [12,-5,-6,50])
    """
    # Initialize first window
    window_sum = sum(nums[:k])
    max_sum = window_sum
    
    # Slide window
    for i in range(k, len(nums)):
        window_sum = window_sum - nums[i - k] + nums[i]
        max_sum = max(max_sum, window_sum)
    
    return max_sum / k

# Test
print(find_max_average([1,12,-5,-6,50,3], 4))  # 12.75
```

### Problem 2: Maximum in Sliding Window

```python
from collections import deque

def max_sliding_window(nums: list[int], k: int) -> list[int]:
    """
    Find maximum in every window of size k.
    
    Time: O(n)
    Space: O(k)
    
    Example: nums = [1,3,-1,-3,5,3,6,7], k = 3
    Output: [3,3,5,5,6,7]
    """
    if not nums:
        return []
    
    result = []
    dq = deque()  # Store indices
    
    for i in range(len(nums)):
        # Remove elements outside window
        while dq and dq[0] < i - k + 1:
            dq.popleft()
        
        # Remove smaller elements (not useful)
        while dq and nums[dq[-1]] < nums[i]:
            dq.pop()
        
        dq.append(i)
        
        # Add to result after first window
        if i >= k - 1:
            result.append(nums[dq[0]])
    
    return result

# Test
print(max_sliding_window([1,3,-1,-3,5,3,6,7], 3))  # [3,3,5,5,6,7]
```

### Problem 3: First Negative in Every Window

```python
from collections import deque

def first_negative_window(arr: list[int], k: int) -> list[int]:
    """
    First negative number in every window of size k.
    
    Time: O(n)
    Space: O(k)
    
    Example: arr = [12, -1, -7, 8, -15, 30, 16, 28], k = 3
    Output: [-1, -1, -7, -15, -15, 0]
    """
    result = []
    negatives = deque()
    
    # First window
    for i in range(k):
        if arr[i] < 0:
            negatives.append(i)
    
    # First result
    result.append(arr[negatives[0]] if negatives else 0)
    
    # Slide window
    for i in range(k, len(arr)):
        # Remove elements outside window
        while negatives and negatives[0] < i - k + 1:
            negatives.popleft()
        
        # Add current if negative
        if arr[i] < 0:
            negatives.append(i)
        
        result.append(arr[negatives[0]] if negatives else 0)
    
    return result

# Test
print(first_negative_window([12, -1, -7, 8, -15, 30, 16, 28], 3))
# [-1, -1, -7, -15, -15, 0]
```

---

## Variable Window

Window size changes based on condition.

### Template

```python
def variable_window(arr, condition):
    """
    Variable window template.
    
    Time: O(n)
    Space: O(1)
    """
    left = 0
    result = 0
    window_state = initialize_state()
    
    for right in range(len(arr)):
        # Expand window
        update_state(arr[right])
        
        # Shrink window while condition violated
        while not valid_window(window_state):
            remove_from_state(arr[left])
            left += 1
        
        # Update result
        result = max(result, right - left + 1)
    
    return result
```

### Problem 4: Longest Substring Without Repeating Characters

```python
def length_of_longest_substring(s: str) -> int:
    """
    Longest substring without repeating characters.
    
    Time: O(n)
    Space: O(min(n, m)) where m is charset size
    
    Example: s = "abcabcbb"
    Output: 3 ("abc")
    """
    char_set = set()
    left = 0
    max_len = 0
    
    for right in range(len(s)):
        # Shrink window until no duplicates
        while s[right] in char_set:
            char_set.remove(s[left])
            left += 1
        
        char_set.add(s[right])
        max_len = max(max_len, right - left + 1)
    
    return max_len

# Test
print(length_of_longest_substring("abcabcbb"))  # 3
print(length_of_longest_substring("bbbbb"))     # 1
print(length_of_longest_substring("pwwkew"))    # 3
```

### Problem 5: Longest Substring with At Most K Distinct Characters

```python
def length_of_longest_substring_k_distinct(s: str, k: int) -> int:
    """
    Longest substring with at most k distinct characters.
    
    Time: O(n)
    Space: O(k)
    
    Example: s = "eceba", k = 2
    Output: 3 ("ece")
    """
    from collections import defaultdict
    
    char_count = defaultdict(int)
    left = 0
    max_len = 0
    
    for right in range(len(s)):
        # Add right character
        char_count[s[right]] += 1
        
        # Shrink while too many distinct
        while len(char_count) > k:
            char_count[s[left]] -= 1
            if char_count[s[left]] == 0:
                del char_count[s[left]]
            left += 1
        
        max_len = max(max_len, right - left + 1)
    
    return max_len

# Test
print(length_of_longest_substring_k_distinct("eceba", 2))  # 3
print(length_of_longest_substring_k_distinct("aa", 1))     # 2
```

### Problem 6: Minimum Window Substring

```python
def min_window(s: str, t: str) -> str:
    """
    Minimum window in s containing all characters of t.
    
    Time: O(n + m)
    Space: O(m)
    
    Example: s = "ADOBECODEBANC", t = "ABC"
    Output: "BANC"
    """
    from collections import Counter
    
    if not s or not t:
        return ""
    
    # Count characters in t
    target_count = Counter(t)
    required = len(target_count)
    
    # Window tracking
    window_count = {}
    formed = 0  # How many unique chars match required frequency
    
    left = 0
    min_len = float('inf')
    min_window = (0, 0)
    
    for right in range(len(s)):
        # Add character from right
        char = s[right]
        window_count[char] = window_count.get(char, 0) + 1
        
        # Check if frequency matches
        if char in target_count and window_count[char] == target_count[char]:
            formed += 1
        
        # Try to shrink window
        while left <= right and formed == required:
            # Update result
            if right - left + 1 < min_len:
                min_len = right - left + 1
                min_window = (left, right)
            
            # Remove from left
            char = s[left]
            window_count[char] -= 1
            if char in target_count and window_count[char] < target_count[char]:
                formed -= 1
            
            left += 1
    
    return "" if min_len == float('inf') else s[min_window[0]:min_window[1] + 1]

# Test
print(min_window("ADOBECODEBANC", "ABC"))  # "BANC"
```

---

## Classic Problems

### Problem 7: Longest Repeating Character Replacement

```python
def character_replacement(s: str, k: int) -> int:
    """
    Longest substring with same characters after k replacements.
    
    Time: O(n)
    Space: O(26) = O(1)
    
    Example: s = "AABABBA", k = 1
    Output: 4 ("AABA" or "ABBB")
    """
    from collections import defaultdict
    
    char_count = defaultdict(int)
    left = 0
    max_len = 0
    max_freq = 0  # Most frequent char in window
    
    for right in range(len(s)):
        char_count[s[right]] += 1
        max_freq = max(max_freq, char_count[s[right]])
        
        # window_length - max_freq = replacements needed
        # If > k, shrink window
        window_len = right - left + 1
        if window_len - max_freq > k:
            char_count[s[left]] -= 1
            left += 1
        
        max_len = max(max_len, right - left + 1)
    
    return max_len

# Test
print(character_replacement("AABABBA", 1))  # 4
print(character_replacement("ABAB", 2))     # 4
```

### Problem 8: Permutation in String

```python
def check_inclusion(s1: str, s2: str) -> bool:
    """
    Check if s2 contains permutation of s1.
    
    Time: O(n)
    Space: O(26) = O(1)
    
    Example: s1 = "ab", s2 = "eidbaooo"
    Output: True (s2 contains "ba")
    """
    from collections import Counter
    
    if len(s1) > len(s2):
        return False
    
    s1_count = Counter(s1)
    window_count = Counter(s2[:len(s1)])
    
    if s1_count == window_count:
        return True
    
    # Slide window
    for i in range(len(s1), len(s2)):
        # Add new character
        window_count[s2[i]] += 1
        
        # Remove old character
        left_char = s2[i - len(s1)]
        window_count[left_char] -= 1
        if window_count[left_char] == 0:
            del window_count[left_char]
        
        if s1_count == window_count:
            return True
    
    return False

# Test
print(check_inclusion("ab", "eidbaooo"))  # True
print(check_inclusion("ab", "eidboaoo"))  # False
```

### Problem 9: Find All Anagrams in String

```python
def find_anagrams(s: str, p: str) -> list[int]:
    """
    Find all start indices of p's anagrams in s.
    
    Time: O(n)
    Space: O(26) = O(1)
    
    Example: s = "cbaebabacd", p = "abc"
    Output: [0, 6] (anagrams "cba" and "bac")
    """
    from collections import Counter
    
    if len(p) > len(s):
        return []
    
    result = []
    p_count = Counter(p)
    window_count = Counter(s[:len(p)])
    
    if p_count == window_count:
        result.append(0)
    
    for i in range(len(p), len(s)):
        # Add new character
        window_count[s[i]] += 1
        
        # Remove old character
        left_char = s[i - len(p)]
        window_count[left_char] -= 1
        if window_count[left_char] == 0:
            del window_count[left_char]
        
        if p_count == window_count:
            result.append(i - len(p) + 1)
    
    return result

# Test
print(find_anagrams("cbaebabacd", "abc"))  # [0, 6]
```

### Problem 10: Maximum Sum Subarray of Size K

```python
def max_sum_subarray(arr: list[int], k: int) -> int:
    """
    Maximum sum of subarray of size k.
    
    Time: O(n)
    Space: O(1)
    
    Example: arr = [2, 1, 5, 1, 3, 2], k = 3
    Output: 9 (subarray [5, 1, 3])
    """
    if len(arr) < k:
        return -1
    
    # First window
    window_sum = sum(arr[:k])
    max_sum = window_sum
    
    # Slide window
    for i in range(k, len(arr)):
        window_sum = window_sum - arr[i - k] + arr[i]
        max_sum = max(max_sum, window_sum)
    
    return max_sum

# Test
print(max_sum_subarray([2, 1, 5, 1, 3, 2], 3))  # 9
```

### Problem 11: Minimum Size Subarray Sum

```python
def min_subarray_len(target: int, nums: list[int]) -> int:
    """
    Minimum length subarray with sum >= target.
    
    Time: O(n)
    Space: O(1)
    
    Example: target = 7, nums = [2,3,1,2,4,3]
    Output: 2 (subarray [4,3])
    """
    left = 0
    min_len = float('inf')
    window_sum = 0
    
    for right in range(len(nums)):
        window_sum += nums[right]
        
        # Shrink while condition met
        while window_sum >= target:
            min_len = min(min_len, right - left + 1)
            window_sum -= nums[left]
            left += 1
    
    return min_len if min_len != float('inf') else 0

# Test
print(min_subarray_len(7, [2,3,1,2,4,3]))  # 2
```

### Problem 12: Longest Subarray with Sum K

```python
def longest_subarray_sum_k(arr: list[int], k: int) -> int:
    """
    Longest subarray with sum equal to k (positive numbers).
    
    Time: O(n)
    Space: O(1)
    
    Example: arr = [1, 2, 3, 1, 1, 1, 1], k = 3
    Output: 3 (subarray [1, 1, 1])
    """
    left = 0
    max_len = 0
    window_sum = 0
    
    for right in range(len(arr)):
        window_sum += arr[right]
        
        # Shrink while sum too large
        while window_sum > k and left <= right:
            window_sum -= arr[left]
            left += 1
        
        # Check if exact match
        if window_sum == k:
            max_len = max(max_len, right - left + 1)
    
    return max_len

# Test
print(longest_subarray_sum_k([1, 2, 3, 1, 1, 1, 1], 3))  # 3
```

### Problem 13: Fruits Into Baskets

```python
def total_fruit(fruits: list[int]) -> int:
    """
    Maximum fruits in two baskets (longest subarray with 2 distinct).
    
    Time: O(n)
    Space: O(1)
    
    Example: fruits = [1,2,1]
    Output: 3
    """
    from collections import defaultdict
    
    fruit_count = defaultdict(int)
    left = 0
    max_fruits = 0
    
    for right in range(len(fruits)):
        fruit_count[fruits[right]] += 1
        
        # Shrink while more than 2 types
        while len(fruit_count) > 2:
            fruit_count[fruits[left]] -= 1
            if fruit_count[fruits[left]] == 0:
                del fruit_count[fruits[left]]
            left += 1
        
        max_fruits = max(max_fruits, right - left + 1)
    
    return max_fruits

# Test
print(total_fruit([1,2,1]))        # 3
print(total_fruit([0,1,2,2]))      # 3
print(total_fruit([1,2,3,2,2]))    # 4
```

### Problem 14: Subarray Product Less Than K

```python
def num_subarray_product_less_than_k(nums: list[int], k: int) -> int:
    """
    Count subarrays with product < k.
    
    Time: O(n)
    Space: O(1)
    
    Example: nums = [10,5,2,6], k = 100
    Output: 8
    """
    if k <= 1:
        return 0
    
    left = 0
    product = 1
    count = 0
    
    for right in range(len(nums)):
        product *= nums[right]
        
        # Shrink while product >= k
        while product >= k:
            product //= nums[left]
            left += 1
        
        # All subarrays ending at right
        count += right - left + 1
    
    return count

# Test
print(num_subarray_product_less_than_k([10,5,2,6], 100))  # 8
```

---

## Interview Tips

### 1. Fixed vs Variable Window

```python
# Fixed Window:
# - Window size given in problem
# - K consecutive elements
# - Every window of size K

# Variable Window:
# - Find optimal window size
# - "Longest/shortest subarray"
# - Condition-based expansion/contraction
```

### 2. Common Window Techniques

```python
# 1. Hash Map for frequency counting
from collections import defaultdict
char_count = defaultdict(int)
for char in window:
    char_count[char] += 1

# 2. Set for uniqueness
char_set = set()
while s[right] in char_set:
    char_set.remove(s[left])
    left += 1

# 3. Deque for maintaining order
from collections import deque
dq = deque()
# Store indices for O(1) max/min

# 4. Two pointers (left, right)
left = 0
for right in range(n):
    # Expand window
    # Shrink if needed
    # Update result
```

### 3. Template Recognition

```python
# Fixed Window Template:
def fixed_window(arr, k):
    window_sum = sum(arr[:k])
    result = window_sum
    for i in range(k, len(arr)):
        window_sum += arr[i] - arr[i-k]
        result = max(result, window_sum)
    return result

# Variable Window Template:
def variable_window(arr):
    left = 0
    result = 0
    for right in range(len(arr)):
        # Add arr[right] to window
        while condition_violated:
            # Remove arr[left] from window
            left += 1
        result = max(result, right - left + 1)
    return result
```

### 4. Optimization Patterns

```python
# From O(n²) to O(n):

# Brute Force O(n²):
for i in range(n):
    for j in range(i, n):
        check_subarray(i, j)

# Sliding Window O(n):
left = 0
for right in range(n):
    # Process right
    while invalid:
        # Process left
        left += 1
```

---

## Practice Problems

### Easy
1. [Maximum Average Subarray I](https://leetcode.com/problems/maximum-average-subarray-i/)
2. [Contains Duplicate II](https://leetcode.com/problems/contains-duplicate-ii/)
3. [Max Consecutive Ones III](https://leetcode.com/problems/max-consecutive-ones-iii/)

### Medium
1. [Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)
2. [Longest Repeating Character Replacement](https://leetcode.com/problems/longest-repeating-character-replacement/)
3. [Permutation in String](https://leetcode.com/problems/permutation-in-string/)
4. [Find All Anagrams in a String](https://leetcode.com/problems/find-all-anagrams-in-a-string/)
5. [Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/)
6. [Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/)
7. [Fruit Into Baskets](https://leetcode.com/problems/fruit-into-baskets/)
8. [Subarray Product Less Than K](https://leetcode.com/problems/subarray-product-less-than-k/)

### Hard
1. [Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/)
2. [Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/)
3. [Longest Substring with At Most K Distinct Characters](https://leetcode.com/problems/longest-substring-with-at-most-k-distinct-characters/)

---

## Summary

### Key Takeaways
- ✅ **Fixed window** - Constant size K
- ✅ **Variable window** - Expand/shrink based on condition
- ✅ **Optimization** - O(n²) → O(n)
- ✅ **Two pointers** - Left and right boundaries

### Quick Reference

```python
# Fixed Window Pattern
window_sum = sum(arr[:k])
for i in range(k, n):
    window_sum += arr[i] - arr[i-k]
    result = max(result, window_sum)

# Variable Window Pattern
left = 0
for right in range(n):
    add_to_window(arr[right])
    while invalid_condition():
        remove_from_window(arr[left])
        left += 1
    update_result()
```

---

**Next**: [Binary Search →](../02-binary-search/README.md)

**Happy Coding! 🚀**
