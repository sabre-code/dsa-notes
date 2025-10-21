# 🎯 Blind 75 - Complete Solutions

> The essential 75 LeetCode problems for coding interviews

---

## 📚 Table of Contents

1. [Array Problems](#array-problems)
2. [Binary Problems](#binary-problems)
3. [Dynamic Programming](#dynamic-programming)
4. [Graph Problems](#graph-problems)
5. [Interval Problems](#interval-problems)
6. [Linked List Problems](#linked-list-problems)
7. [Matrix Problems](#matrix-problems)
8. [String Problems](#string-problems)
9. [Tree Problems](#tree-problems)
10. [Heap Problems](#heap-problems)

---

## Array Problems

### 1. Two Sum
**LeetCode #1** | Easy | ⭐⭐⭐

```python
def two_sum(nums: list[int], target: int) -> list[int]:
    """
    Find two numbers that add up to target.
    
    Time: O(n)
    Space: O(n)
    
    Example: nums = [2,7,11,15], target = 9
    Output: [0,1]
    """
    seen = {}
    
    for i, num in enumerate(nums):
        complement = target - num
        if complement in seen:
            return [seen[complement], i]
        seen[num] = i
    
    return []
```

### 2. Best Time to Buy and Sell Stock
**LeetCode #121** | Easy | ⭐⭐⭐

```python
def max_profit(prices: list[int]) -> int:
    """
    Find maximum profit from one transaction.
    
    Time: O(n)
    Space: O(1)
    
    Example: prices = [7,1,5,3,6,4]
    Output: 5 (buy at 1, sell at 6)
    """
    min_price = float('inf')
    max_profit = 0
    
    for price in prices:
        min_price = min(min_price, price)
        max_profit = max(max_profit, price - min_price)
    
    return max_profit
```

### 3. Contains Duplicate
**LeetCode #217** | Easy | ⭐⭐

```python
def contains_duplicate(nums: list[int]) -> bool:
    """
    Check if array contains duplicates.
    
    Time: O(n)
    Space: O(n)
    
    Example: nums = [1,2,3,1]
    Output: True
    """
    return len(nums) != len(set(nums))
```

### 4. Product of Array Except Self
**LeetCode #238** | Medium | ⭐⭐⭐

```python
def product_except_self(nums: list[int]) -> list[int]:
    """
    Return array where answer[i] is product of all elements except nums[i].
    
    Time: O(n)
    Space: O(1) (output array doesn't count)
    
    Example: nums = [1,2,3,4]
    Output: [24,12,8,6]
    """
    n = len(nums)
    result = [1] * n
    
    # Calculate prefix products
    prefix = 1
    for i in range(n):
        result[i] = prefix
        prefix *= nums[i]
    
    # Calculate suffix products and multiply
    suffix = 1
    for i in range(n - 1, -1, -1):
        result[i] *= suffix
        suffix *= nums[i]
    
    return result
```

### 5. Maximum Subarray
**LeetCode #53** | Medium | ⭐⭐⭐

```python
def max_subarray(nums: list[int]) -> int:
    """
    Find contiguous subarray with largest sum (Kadane's Algorithm).
    
    Time: O(n)
    Space: O(1)
    
    Example: nums = [-2,1,-3,4,-1,2,1,-5,4]
    Output: 6 ([4,-1,2,1])
    """
    max_sum = current_sum = nums[0]
    
    for num in nums[1:]:
        current_sum = max(num, current_sum + num)
        max_sum = max(max_sum, current_sum)
    
    return max_sum
```

### 6. Maximum Product Subarray
**LeetCode #152** | Medium | ⭐⭐⭐

```python
def max_product(nums: list[int]) -> int:
    """
    Find contiguous subarray with largest product.
    
    Time: O(n)
    Space: O(1)
    
    Example: nums = [2,3,-2,4]
    Output: 6 ([2,3])
    """
    max_prod = min_prod = result = nums[0]
    
    for num in nums[1:]:
        if num < 0:
            max_prod, min_prod = min_prod, max_prod
        
        max_prod = max(num, max_prod * num)
        min_prod = min(num, min_prod * num)
        result = max(result, max_prod)
    
    return result
```

### 7. Find Minimum in Rotated Sorted Array
**LeetCode #153** | Medium | ⭐⭐⭐

```python
def find_min(nums: list[int]) -> int:
    """
    Find minimum in rotated sorted array.
    
    Time: O(log n)
    Space: O(1)
    
    Example: nums = [3,4,5,1,2]
    Output: 1
    """
    left, right = 0, len(nums) - 1
    
    while left < right:
        mid = (left + right) // 2
        
        if nums[mid] > nums[right]:
            left = mid + 1
        else:
            right = mid
    
    return nums[left]
```

### 8. Search in Rotated Sorted Array
**LeetCode #33** | Medium | ⭐⭐⭐

```python
def search(nums: list[int], target: int) -> int:
    """
    Search target in rotated sorted array.
    
    Time: O(log n)
    Space: O(1)
    
    Example: nums = [4,5,6,7,0,1,2], target = 0
    Output: 4
    """
    left, right = 0, len(nums) - 1
    
    while left <= right:
        mid = (left + right) // 2
        
        if nums[mid] == target:
            return mid
        
        # Left half is sorted
        if nums[left] <= nums[mid]:
            if nums[left] <= target < nums[mid]:
                right = mid - 1
            else:
                left = mid + 1
        # Right half is sorted
        else:
            if nums[mid] < target <= nums[right]:
                left = mid + 1
            else:
                right = mid - 1
    
    return -1
```

### 9. 3Sum
**LeetCode #15** | Medium | ⭐⭐⭐

```python
def three_sum(nums: list[int]) -> list[list[int]]:
    """
    Find all triplets that sum to zero.
    
    Time: O(n²)
    Space: O(1)
    
    Example: nums = [-1,0,1,2,-1,-4]
    Output: [[-1,-1,2],[-1,0,1]]
    """
    nums.sort()
    result = []
    
    for i in range(len(nums) - 2):
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
                
                while left < right and nums[left] == nums[left+1]:
                    left += 1
                while left < right and nums[right] == nums[right-1]:
                    right -= 1
                
                left += 1
                right -= 1
    
    return result
```

### 10. Container With Most Water
**LeetCode #11** | Medium | ⭐⭐⭐

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
        
        if height[left] < height[right]:
            left += 1
        else:
            right -= 1
    
    return max_water
```

---

## Binary Problems

### 11. Sum of Two Integers
**LeetCode #371** | Medium | ⭐⭐

```python
def get_sum(a: int, b: int) -> int:
    """
    Add two integers without using + or -.
    
    Time: O(1)
    Space: O(1)
    
    Example: a = 1, b = 2
    Output: 3
    """
    mask = 0xFFFFFFFF
    
    while b != 0:
        sum_without_carry = (a ^ b) & mask
        carry = ((a & b) << 1) & mask
        a = sum_without_carry
        b = carry
    
    return a if a <= 0x7FFFFFFF else ~(a ^ mask)
```

### 12. Number of 1 Bits
**LeetCode #191** | Easy | ⭐⭐

```python
def hamming_weight(n: int) -> int:
    """
    Count number of 1 bits.
    
    Time: O(1)
    Space: O(1)
    
    Example: n = 11 (binary: 1011)
    Output: 3
    """
    count = 0
    while n:
        n &= n - 1
        count += 1
    return count
```

### 13. Counting Bits
**LeetCode #338** | Easy | ⭐⭐

```python
def count_bits(n: int) -> list[int]:
    """
    Count 1 bits for numbers 0 to n.
    
    Time: O(n)
    Space: O(1)
    
    Example: n = 5
    Output: [0,1,1,2,1,2]
    """
    result = [0] * (n + 1)
    
    for i in range(1, n + 1):
        result[i] = result[i >> 1] + (i & 1)
    
    return result
```

### 14. Missing Number
**LeetCode #268** | Easy | ⭐⭐

```python
def missing_number(nums: list[int]) -> int:
    """
    Find missing number in [0,n].
    
    Time: O(n)
    Space: O(1)
    
    Example: nums = [3,0,1]
    Output: 2
    """
    n = len(nums)
    return n * (n + 1) // 2 - sum(nums)
```

### 15. Reverse Bits
**LeetCode #190** | Easy | ⭐⭐

```python
def reverse_bits(n: int) -> int:
    """
    Reverse bits of 32-bit integer.
    
    Time: O(1)
    Space: O(1)
    
    Example: n = 43261596
    Output: 964176192
    """
    result = 0
    for _ in range(32):
        result = (result << 1) | (n & 1)
        n >>= 1
    return result
```

---

## Dynamic Programming

### 16. Climbing Stairs
**LeetCode #70** | Easy | ⭐⭐⭐

```python
def climb_stairs(n: int) -> int:
    """
    Count ways to climb n stairs (1 or 2 steps).
    
    Time: O(n)
    Space: O(1)
    
    Example: n = 3
    Output: 3
    """
    if n <= 2:
        return n
    
    prev2, prev1 = 1, 2
    
    for _ in range(3, n + 1):
        current = prev1 + prev2
        prev2, prev1 = prev1, current
    
    return prev1
```

### 17. Coin Change
**LeetCode #322** | Medium | ⭐⭐⭐

```python
def coin_change(coins: list[int], amount: int) -> int:
    """
    Find minimum coins needed for amount.
    
    Time: O(amount × len(coins))
    Space: O(amount)
    
    Example: coins = [1,2,5], amount = 11
    Output: 3
    """
    dp = [float('inf')] * (amount + 1)
    dp[0] = 0
    
    for coin in coins:
        for i in range(coin, amount + 1):
            dp[i] = min(dp[i], dp[i - coin] + 1)
    
    return dp[amount] if dp[amount] != float('inf') else -1
```

### 18. Longest Increasing Subsequence
**LeetCode #300** | Medium | ⭐⭐⭐

```python
def length_of_lis(nums: list[int]) -> int:
    """
    Find length of longest increasing subsequence.
    
    Time: O(n log n)
    Space: O(n)
    
    Example: nums = [10,9,2,5,3,7,101,18]
    Output: 4
    """
    import bisect
    
    sub = []
    
    for num in nums:
        pos = bisect.bisect_left(sub, num)
        if pos == len(sub):
            sub.append(num)
        else:
            sub[pos] = num
    
    return len(sub)
```

### 19. Longest Common Subsequence
**LeetCode #1143** | Medium | ⭐⭐⭐

```python
def longest_common_subsequence(text1: str, text2: str) -> int:
    """
    Find length of longest common subsequence.
    
    Time: O(m × n)
    Space: O(m × n)
    
    Example: text1 = "abcde", text2 = "ace"
    Output: 3
    """
    m, n = len(text1), len(text2)
    dp = [[0] * (n + 1) for _ in range(m + 1)]
    
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if text1[i-1] == text2[j-1]:
                dp[i][j] = dp[i-1][j-1] + 1
            else:
                dp[i][j] = max(dp[i-1][j], dp[i][j-1])
    
    return dp[m][n]
```

### 20. Word Break
**LeetCode #139** | Medium | ⭐⭐⭐

```python
def word_break(s: str, word_dict: list[str]) -> bool:
    """
    Check if string can be segmented into words.
    
    Time: O(n² × m)
    Space: O(n)
    
    Example: s = "leetcode", wordDict = ["leet","code"]
    Output: True
    """
    word_set = set(word_dict)
    dp = [False] * (len(s) + 1)
    dp[0] = True
    
    for i in range(1, len(s) + 1):
        for j in range(i):
            if dp[j] and s[j:i] in word_set:
                dp[i] = True
                break
    
    return dp[len(s)]
```

### 21. Combination Sum IV
**LeetCode #377** | Medium | ⭐⭐

```python
def combination_sum4(nums: list[int], target: int) -> int:
    """
    Count combinations that sum to target.
    
    Time: O(target × len(nums))
    Space: O(target)
    
    Example: nums = [1,2,3], target = 4
    Output: 7
    """
    dp = [0] * (target + 1)
    dp[0] = 1
    
    for i in range(1, target + 1):
        for num in nums:
            if i >= num:
                dp[i] += dp[i - num]
    
    return dp[target]
```

### 22. House Robber
**LeetCode #198** | Medium | ⭐⭐⭐

```python
def rob(nums: list[int]) -> int:
    """
    Maximum money to rob (can't rob adjacent).
    
    Time: O(n)
    Space: O(1)
    
    Example: nums = [1,2,3,1]
    Output: 4
    """
    prev2, prev1 = 0, 0
    
    for num in nums:
        current = max(prev1, prev2 + num)
        prev2, prev1 = prev1, current
    
    return prev1
```

### 23. House Robber II
**LeetCode #213** | Medium | ⭐⭐⭐

```python
def rob2(nums: list[int]) -> int:
    """
    Maximum money to rob (circular).
    
    Time: O(n)
    Space: O(1)
    
    Example: nums = [2,3,2]
    Output: 3
    """
    def rob_linear(houses):
        prev2, prev1 = 0, 0
        for num in houses:
            current = max(prev1, prev2 + num)
            prev2, prev1 = prev1, current
        return prev1
    
    if len(nums) == 1:
        return nums[0]
    
    return max(rob_linear(nums[:-1]), rob_linear(nums[1:]))
```

### 24. Decode Ways
**LeetCode #91** | Medium | ⭐⭐⭐

```python
def num_decodings(s: str) -> int:
    """
    Count ways to decode string.
    
    Time: O(n)
    Space: O(1)
    
    Example: s = "226"
    Output: 3
    """
    if not s or s[0] == '0':
        return 0
    
    prev2, prev1 = 1, 1
    
    for i in range(1, len(s)):
        current = 0
        
        if s[i] != '0':
            current += prev1
        
        two_digit = int(s[i-1:i+1])
        if 10 <= two_digit <= 26:
            current += prev2
        
        prev2, prev1 = prev1, current
    
    return prev1
```

### 25. Unique Paths
**LeetCode #62** | Medium | ⭐⭐⭐

```python
def unique_paths(m: int, n: int) -> int:
    """
    Count unique paths in m×n grid.
    
    Time: O(m × n)
    Space: O(n)
    
    Example: m = 3, n = 7
    Output: 28
    """
    dp = [1] * n
    
    for _ in range(1, m):
        for j in range(1, n):
            dp[j] += dp[j-1]
    
    return dp[-1]
```

### 26. Jump Game
**LeetCode #55** | Medium | ⭐⭐⭐

```python
def can_jump(nums: list[int]) -> bool:
    """
    Check if can reach last index.
    
    Time: O(n)
    Space: O(1)
    
    Example: nums = [2,3,1,1,4]
    Output: True
    """
    max_reach = 0
    
    for i, jump in enumerate(nums):
        if i > max_reach:
            return False
        max_reach = max(max_reach, i + jump)
    
    return True
```

---

## String Problems

### 27. Longest Substring Without Repeating Characters
**LeetCode #3** | Medium | ⭐⭐⭐

```python
def length_of_longest_substring(s: str) -> int:
    """
    Length of longest substring without repeating characters.
    
    Time: O(n)
    Space: O(min(n, charset))
    
    Example: s = "abcabcbb"
    Output: 3
    """
    char_index = {}
    max_length = start = 0
    
    for end, char in enumerate(s):
        if char in char_index and char_index[char] >= start:
            start = char_index[char] + 1
        
        char_index[char] = end
        max_length = max(max_length, end - start + 1)
    
    return max_length
```

### 28. Longest Repeating Character Replacement
**LeetCode #424** | Medium | ⭐⭐⭐

```python
def character_replacement(s: str, k: int) -> int:
    """
    Longest substring with same letter after k replacements.
    
    Time: O(n)
    Space: O(1)
    
    Example: s = "AABABBA", k = 1
    Output: 4
    """
    count = {}
    max_freq = max_length = start = 0
    
    for end, char in enumerate(s):
        count[char] = count.get(char, 0) + 1
        max_freq = max(max_freq, count[char])
        
        if (end - start + 1) - max_freq > k:
            count[s[start]] -= 1
            start += 1
        
        max_length = max(max_length, end - start + 1)
    
    return max_length
```

### 29. Minimum Window Substring
**LeetCode #76** | Hard | ⭐⭐⭐

```python
def min_window(s: str, t: str) -> str:
    """
    Minimum window substring containing all characters of t.
    
    Time: O(m + n)
    Space: O(m + n)
    
    Example: s = "ADOBECODEBANC", t = "ABC"
    Output: "BANC"
    """
    from collections import Counter
    
    if not s or not t:
        return ""
    
    target_count = Counter(t)
    required = len(target_count)
    formed = 0
    
    window_count = {}
    left = 0
    min_len = float('inf')
    min_window = (0, 0)
    
    for right, char in enumerate(s):
        window_count[char] = window_count.get(char, 0) + 1
        
        if char in target_count and window_count[char] == target_count[char]:
            formed += 1
        
        while left <= right and formed == required:
            if right - left + 1 < min_len:
                min_len = right - left + 1
                min_window = (left, right)
            
            window_count[s[left]] -= 1
            if s[left] in target_count and window_count[s[left]] < target_count[s[left]]:
                formed -= 1
            
            left += 1
    
    return "" if min_len == float('inf') else s[min_window[0]:min_window[1]+1]
```

### 30. Valid Anagram
**LeetCode #242** | Easy | ⭐⭐

```python
def is_anagram(s: str, t: str) -> bool:
    """
    Check if t is anagram of s.
    
    Time: O(n)
    Space: O(1)
    
    Example: s = "anagram", t = "nagaram"
    Output: True
    """
    from collections import Counter
    return Counter(s) == Counter(t)
```

### 31. Group Anagrams
**LeetCode #49** | Medium | ⭐⭐⭐

```python
def group_anagrams(strs: list[str]) -> list[list[str]]:
    """
    Group anagrams together.
    
    Time: O(n × k log k)
    Space: O(n × k)
    
    Example: strs = ["eat","tea","tan","ate","nat","bat"]
    Output: [["bat"],["nat","tan"],["ate","eat","tea"]]
    """
    from collections import defaultdict
    
    groups = defaultdict(list)
    
    for s in strs:
        key = ''.join(sorted(s))
        groups[key].append(s)
    
    return list(groups.values())
```

### 32. Valid Parentheses
**LeetCode #20** | Easy | ⭐⭐⭐

```python
def is_valid(s: str) -> bool:
    """
    Check if parentheses are valid.
    
    Time: O(n)
    Space: O(n)
    
    Example: s = "()[]{}"
    Output: True
    """
    stack = []
    mapping = {')': '(', '}': '{', ']': '['}
    
    for char in s:
        if char in mapping:
            top = stack.pop() if stack else '#'
            if mapping[char] != top:
                return False
        else:
            stack.append(char)
    
    return not stack
```

### 33. Valid Palindrome
**LeetCode #125** | Easy | ⭐⭐

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
        while left < right and not s[left].isalnum():
            left += 1
        while left < right and not s[right].isalnum():
            right -= 1
        
        if s[left].lower() != s[right].lower():
            return False
        
        left += 1
        right -= 1
    
    return True
```

### 34. Longest Palindromic Substring
**LeetCode #5** | Medium | ⭐⭐⭐

```python
def longest_palindrome(s: str) -> str:
    """
    Find longest palindromic substring.
    
    Time: O(n²)
    Space: O(1)
    
    Example: s = "babad"
    Output: "bab"
    """
    def expand(left, right):
        while left >= 0 and right < len(s) and s[left] == s[right]:
            left -= 1
            right += 1
        return s[left+1:right]
    
    longest = ""
    
    for i in range(len(s)):
        odd = expand(i, i)
        even = expand(i, i + 1)
        
        longest = max(longest, odd, even, key=len)
    
    return longest
```

### 35. Palindromic Substrings
**LeetCode #647** | Medium | ⭐⭐⭐

```python
def count_substrings(s: str) -> int:
    """
    Count palindromic substrings.
    
    Time: O(n²)
    Space: O(1)
    
    Example: s = "abc"
    Output: 3
    """
    def expand(left, right):
        count = 0
        while left >= 0 and right < len(s) and s[left] == s[right]:
            count += 1
            left -= 1
            right += 1
        return count
    
    total = 0
    for i in range(len(s)):
        total += expand(i, i)
        total += expand(i, i + 1)
    
    return total
```

### 36. Encode and Decode Strings
**LeetCode #271** | Medium | ⭐⭐⭐

```python
def encode(strs: list[str]) -> str:
    """Encode list to string."""
    return ''.join(f"{len(s)}#{s}" for s in strs)

def decode(s: str) -> list[str]:
    """Decode string to list."""
    result = []
    i = 0
    
    while i < len(s):
        j = s.index('#', i)
        length = int(s[i:j])
        result.append(s[j+1:j+1+length])
        i = j + 1 + length
    
    return result
```

---

## Linked List Problems

### 37. Reverse Linked List
**LeetCode #206** | Easy | ⭐⭐⭐

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def reverse_list(head: ListNode) -> ListNode:
    """
    Reverse linked list.
    
    Time: O(n)
    Space: O(1)
    """
    prev = None
    current = head
    
    while current:
        next_node = current.next
        current.next = prev
        prev = current
        current = next_node
    
    return prev
```

### 38. Linked List Cycle
**LeetCode #141** | Easy | ⭐⭐⭐

```python
def has_cycle(head: ListNode) -> bool:
    """
    Detect cycle using fast & slow pointers.
    
    Time: O(n)
    Space: O(1)
    """
    slow = fast = head
    
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        
        if slow == fast:
            return True
    
    return False
```

### 39. Merge Two Sorted Lists
**LeetCode #21** | Easy | ⭐⭐⭐

```python
def merge_two_lists(l1: ListNode, l2: ListNode) -> ListNode:
    """
    Merge two sorted lists.
    
    Time: O(m + n)
    Space: O(1)
    """
    dummy = ListNode(0)
    current = dummy
    
    while l1 and l2:
        if l1.val < l2.val:
            current.next = l1
            l1 = l1.next
        else:
            current.next = l2
            l2 = l2.next
        current = current.next
    
    current.next = l1 or l2
    
    return dummy.next
```

### 40. Merge K Sorted Lists
**LeetCode #23** | Hard | ⭐⭐⭐

```python
def merge_k_lists(lists: list[ListNode]) -> ListNode:
    """
    Merge k sorted lists using min heap.
    
    Time: O(n log k)
    Space: O(k)
    """
    import heapq
    
    heap = []
    
    for i, node in enumerate(lists):
        if node:
            heapq.heappush(heap, (node.val, i, node))
    
    dummy = ListNode(0)
    current = dummy
    
    while heap:
        val, i, node = heapq.heappop(heap)
        current.next = node
        current = current.next
        
        if node.next:
            heapq.heappush(heap, (node.next.val, i, node.next))
    
    return dummy.next
```

### 41. Remove Nth Node From End
**LeetCode #19** | Medium | ⭐⭐⭐

```python
def remove_nth_from_end(head: ListNode, n: int) -> ListNode:
    """
    Remove nth node from end.
    
    Time: O(n)
    Space: O(1)
    """
    dummy = ListNode(0)
    dummy.next = head
    slow = fast = dummy
    
    for _ in range(n + 1):
        fast = fast.next
    
    while fast:
        slow = slow.next
        fast = fast.next
    
    slow.next = slow.next.next
    
    return dummy.next
```

### 42. Reorder List
**LeetCode #143** | Medium | ⭐⭐⭐

```python
def reorder_list(head: ListNode) -> None:
    """
    Reorder: L0→Ln→L1→Ln-1→L2→Ln-2→...
    
    Time: O(n)
    Space: O(1)
    """
    if not head or not head.next:
        return
    
    # Find middle
    slow = fast = head
    while fast.next and fast.next.next:
        slow = slow.next
        fast = fast.next.next
    
    # Reverse second half
    second = slow.next
    slow.next = None
    prev = None
    
    while second:
        next_node = second.next
        second.next = prev
        prev = second
        second = next_node
    
    # Merge
    first, second = head, prev
    while second:
        next1, next2 = first.next, second.next
        first.next = second
        second.next = next1
        first, second = next1, next2
```

---

## Tree Problems

### 43. Maximum Depth of Binary Tree
**LeetCode #104** | Easy | ⭐⭐

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def max_depth(root: TreeNode) -> int:
    """
    Find maximum depth of binary tree.
    
    Time: O(n)
    Space: O(h)
    """
    if not root:
        return 0
    
    return 1 + max(max_depth(root.left), max_depth(root.right))
```

### 44. Same Tree
**LeetCode #100** | Easy | ⭐⭐

```python
def is_same_tree(p: TreeNode, q: TreeNode) -> bool:
    """
    Check if two trees are identical.
    
    Time: O(n)
    Space: O(h)
    """
    if not p and not q:
        return True
    if not p or not q:
        return False
    
    return (p.val == q.val and 
            is_same_tree(p.left, q.left) and 
            is_same_tree(p.right, q.right))
```

### 45. Invert Binary Tree
**LeetCode #226** | Easy | ⭐⭐⭐

```python
def invert_tree(root: TreeNode) -> TreeNode:
    """
    Invert binary tree.
    
    Time: O(n)
    Space: O(h)
    """
    if not root:
        return None
    
    root.left, root.right = root.right, root.left
    invert_tree(root.left)
    invert_tree(root.right)
    
    return root
```

### 46. Binary Tree Maximum Path Sum
**LeetCode #124** | Hard | ⭐⭐⭐

```python
def max_path_sum(root: TreeNode) -> int:
    """
    Find maximum path sum in binary tree.
    
    Time: O(n)
    Space: O(h)
    """
    max_sum = float('-inf')
    
    def dfs(node):
        nonlocal max_sum
        
        if not node:
            return 0
        
        left = max(0, dfs(node.left))
        right = max(0, dfs(node.right))
        
        max_sum = max(max_sum, left + right + node.val)
        
        return node.val + max(left, right)
    
    dfs(root)
    return max_sum
```

### 47. Binary Tree Level Order Traversal
**LeetCode #102** | Medium | ⭐⭐⭐

```python
def level_order(root: TreeNode) -> list[list[int]]:
    """
    Level order traversal (BFS).
    
    Time: O(n)
    Space: O(w) where w is max width
    """
    if not root:
        return []
    
    from collections import deque
    
    result = []
    queue = deque([root])
    
    while queue:
        level = []
        
        for _ in range(len(queue)):
            node = queue.popleft()
            level.append(node.val)
            
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)
        
        result.append(level)
    
    return result
```

### 48. Serialize and Deserialize Binary Tree
**LeetCode #297** | Hard | ⭐⭐⭐

```python
class Codec:
    """
    Serialize and deserialize binary tree.
    
    Time: O(n)
    Space: O(n)
    """
    
    def serialize(self, root: TreeNode) -> str:
        """Encode tree to string."""
        def dfs(node):
            if not node:
                return ['null']
            
            return [str(node.val)] + dfs(node.left) + dfs(node.right)
        
        return ','.join(dfs(root))
    
    def deserialize(self, data: str) -> TreeNode:
        """Decode string to tree."""
        def dfs():
            val = next(vals)
            
            if val == 'null':
                return None
            
            node = TreeNode(int(val))
            node.left = dfs()
            node.right = dfs()
            
            return node
        
        vals = iter(data.split(','))
        return dfs()
```

### 49. Subtree of Another Tree
**LeetCode #572** | Easy | ⭐⭐

```python
def is_subtree(root: TreeNode, subRoot: TreeNode) -> bool:
    """
    Check if subRoot is subtree of root.
    
    Time: O(m × n)
    Space: O(h)
    """
    def is_same(p, q):
        if not p and not q:
            return True
        if not p or not q:
            return False
        return (p.val == q.val and 
                is_same(p.left, q.left) and 
                is_same(p.right, q.right))
    
    if not root:
        return False
    
    if is_same(root, subRoot):
        return True
    
    return is_subtree(root.left, subRoot) or is_subtree(root.right, subRoot)
```

### 50. Construct Binary Tree from Preorder and Inorder
**LeetCode #105** | Medium | ⭐⭐⭐

```python
def build_tree(preorder: list[int], inorder: list[int]) -> TreeNode:
    """
    Construct tree from preorder and inorder traversals.
    
    Time: O(n)
    Space: O(n)
    """
    if not preorder or not inorder:
        return None
    
    root = TreeNode(preorder[0])
    mid = inorder.index(preorder[0])
    
    root.left = build_tree(preorder[1:mid+1], inorder[:mid])
    root.right = build_tree(preorder[mid+1:], inorder[mid+1:])
    
    return root
```

### 51. Validate Binary Search Tree
**LeetCode #98** | Medium | ⭐⭐⭐

```python
def is_valid_bst(root: TreeNode) -> bool:
    """
    Validate binary search tree.
    
    Time: O(n)
    Space: O(h)
    """
    def validate(node, min_val, max_val):
        if not node:
            return True
        
        if not (min_val < node.val < max_val):
            return False
        
        return (validate(node.left, min_val, node.val) and 
                validate(node.right, node.val, max_val))
    
    return validate(root, float('-inf'), float('inf'))
```

### 52. Kth Smallest Element in BST
**LeetCode #230** | Medium | ⭐⭐⭐

```python
def kth_smallest(root: TreeNode, k: int) -> int:
    """
    Find kth smallest element in BST.
    
    Time: O(h + k)
    Space: O(h)
    """
    stack = []
    current = root
    count = 0
    
    while stack or current:
        while current:
            stack.append(current)
            current = current.left
        
        current = stack.pop()
        count += 1
        
        if count == k:
            return current.val
        
        current = current.right
```

### 53. Lowest Common Ancestor of BST
**LeetCode #235** | Easy | ⭐⭐⭐

```python
def lowest_common_ancestor(root: TreeNode, p: TreeNode, q: TreeNode) -> TreeNode:
    """
    Find LCA in BST.
    
    Time: O(h)
    Space: O(1)
    """
    while root:
        if p.val < root.val and q.val < root.val:
            root = root.left
        elif p.val > root.val and q.val > root.val:
            root = root.right
        else:
            return root
```

### 54. Implement Trie (Prefix Tree)
**LeetCode #208** | Medium | ⭐⭐⭐

```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_end = False

class Trie:
    """
    Implement trie (prefix tree).
    
    Time: O(m) per operation where m is word length
    Space: O(total characters)
    """
    
    def __init__(self):
        self.root = TrieNode()
    
    def insert(self, word: str) -> None:
        """Insert word into trie."""
        node = self.root
        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
        node.is_end = True
    
    def search(self, word: str) -> bool:
        """Search for word in trie."""
        node = self.root
        for char in word:
            if char not in node.children:
                return False
            node = node.children[char]
        return node.is_end
    
    def starts_with(self, prefix: str) -> bool:
        """Check if prefix exists."""
        node = self.root
        for char in prefix:
            if char not in node.children:
                return False
            node = node.children[char]
        return True
```

### 55. Design Add and Search Words Data Structure
**LeetCode #211** | Medium | ⭐⭐⭐

```python
class WordDictionary:
    """
    Add and search words (support '.' wildcard).
    
    Time: O(m) insert, O(26^m) search worst case
    Space: O(total characters)
    """
    
    def __init__(self):
        self.root = TrieNode()
    
    def add_word(self, word: str) -> None:
        """Add word to dictionary."""
        node = self.root
        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
        node.is_end = True
    
    def search(self, word: str) -> bool:
        """Search word (support '.' wildcard)."""
        def dfs(node, i):
            if i == len(word):
                return node.is_end
            
            if word[i] == '.':
                for child in node.children.values():
                    if dfs(child, i + 1):
                        return True
                return False
            else:
                if word[i] not in node.children:
                    return False
                return dfs(node.children[word[i]], i + 1)
        
        return dfs(self.root, 0)
```

### 56. Word Search II
**LeetCode #212** | Hard | ⭐⭐⭐

```python
def find_words(board: list[list[str]], words: list[str]) -> list[str]:
    """
    Find all words from list in board.
    
    Time: O(m × n × 4^L) where L is max word length
    Space: O(total characters in words)
    """
    # Build trie
    root = TrieNode()
    for word in words:
        node = root
        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
        node.is_end = True
        node.word = word
    
    result = []
    m, n = len(board), len(board[0])
    
    def dfs(i, j, node):
        if node.is_end:
            result.append(node.word)
            node.is_end = False  # Avoid duplicates
        
        if i < 0 or i >= m or j < 0 or j >= n:
            return
        
        char = board[i][j]
        if char not in node.children:
            return
        
        board[i][j] = '#'
        
        for di, dj in [(0,1), (1,0), (0,-1), (-1,0)]:
            dfs(i + di, j + dj, node.children[char])
        
        board[i][j] = char
    
    for i in range(m):
        for j in range(n):
            dfs(i, j, root)
    
    return result
```

---

## Graph Problems

### 57. Clone Graph
**LeetCode #133** | Medium | ⭐⭐⭐

```python
class Node:
    def __init__(self, val=0, neighbors=None):
        self.val = val
        self.neighbors = neighbors if neighbors is not None else []

def clone_graph(node: Node) -> Node:
    """
    Clone graph.
    
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

### 58. Course Schedule
**LeetCode #207** | Medium | ⭐⭐⭐

```python
def can_finish(num_courses: int, prerequisites: list[list[int]]) -> bool:
    """
    Check if can finish all courses (detect cycle).
    
    Time: O(V + E)
    Space: O(V + E)
    """
    from collections import defaultdict, deque
    
    # Build graph
    graph = defaultdict(list)
    in_degree = [0] * num_courses
    
    for course, prereq in prerequisites:
        graph[prereq].append(course)
        in_degree[course] += 1
    
    # Topological sort
    queue = deque([i for i in range(num_courses) if in_degree[i] == 0])
    count = 0
    
    while queue:
        course = queue.popleft()
        count += 1
        
        for next_course in graph[course]:
            in_degree[next_course] -= 1
            if in_degree[next_course] == 0:
                queue.append(next_course)
    
    return count == num_courses
```

### 59. Pacific Atlantic Water Flow
**LeetCode #417** | Medium | ⭐⭐⭐

```python
def pacific_atlantic(heights: list[list[int]]) -> list[list[int]]:
    """
    Find cells that can flow to both oceans.
    
    Time: O(m × n)
    Space: O(m × n)
    """
    if not heights:
        return []
    
    m, n = len(heights), len(heights[0])
    pacific = set()
    atlantic = set()
    
    def dfs(i, j, visited):
        visited.add((i, j))
        
        for di, dj in [(0,1), (1,0), (0,-1), (-1,0)]:
            ni, nj = i + di, j + dj
            
            if (0 <= ni < m and 0 <= nj < n and 
                (ni, nj) not in visited and 
                heights[ni][nj] >= heights[i][j]):
                dfs(ni, nj, visited)
    
    # DFS from Pacific border
    for i in range(m):
        dfs(i, 0, pacific)
    for j in range(n):
        dfs(0, j, pacific)
    
    # DFS from Atlantic border
    for i in range(m):
        dfs(i, n - 1, atlantic)
    for j in range(n):
        dfs(m - 1, j, atlantic)
    
    return list(pacific & atlantic)
```

### 60. Number of Islands
**LeetCode #200** | Medium | ⭐⭐⭐

```python
def num_islands(grid: list[list[str]]) -> int:
    """
    Count number of islands.
    
    Time: O(m × n)
    Space: O(m × n)
    """
    if not grid:
        return 0
    
    m, n = len(grid), len(grid[0])
    count = 0
    
    def dfs(i, j):
        if i < 0 or i >= m or j < 0 or j >= n or grid[i][j] != '1':
            return
        
        grid[i][j] = '#'
        
        dfs(i + 1, j)
        dfs(i - 1, j)
        dfs(i, j + 1)
        dfs(i, j - 1)
    
    for i in range(m):
        for j in range(n):
            if grid[i][j] == '1':
                dfs(i, j)
                count += 1
    
    return count
```

### 61. Longest Consecutive Sequence
**LeetCode #128** | Medium | ⭐⭐⭐

```python
def longest_consecutive(nums: list[int]) -> int:
    """
    Find length of longest consecutive sequence.
    
    Time: O(n)
    Space: O(n)
    
    Example: nums = [100,4,200,1,3,2]
    Output: 4 ([1,2,3,4])
    """
    num_set = set(nums)
    max_length = 0
    
    for num in num_set:
        # Only start sequence from beginning
        if num - 1 not in num_set:
            current = num
            length = 1
            
            while current + 1 in num_set:
                current += 1
                length += 1
            
            max_length = max(max_length, length)
    
    return max_length
```

### 62. Graph Valid Tree
**LeetCode #261** | Medium | ⭐⭐⭐

```python
def valid_tree(n: int, edges: list[list[int]]) -> bool:
    """
    Check if graph is valid tree.
    
    Time: O(V + E)
    Space: O(V + E)
    
    Valid tree: n-1 edges, no cycles, connected
    """
    if len(edges) != n - 1:
        return False
    
    from collections import defaultdict
    
    graph = defaultdict(list)
    for a, b in edges:
        graph[a].append(b)
        graph[b].append(a)
    
    visited = set()
    
    def dfs(node, parent):
        visited.add(node)
        
        for neighbor in graph[node]:
            if neighbor == parent:
                continue
            if neighbor in visited:
                return False
            if not dfs(neighbor, node):
                return False
        
        return True
    
    return dfs(0, -1) and len(visited) == n
```

### 63. Number of Connected Components
**LeetCode #323** | Medium | ⭐⭐⭐

```python
def count_components(n: int, edges: list[list[int]]) -> int:
    """
    Count number of connected components.
    
    Time: O(V + E)
    Space: O(V)
    """
    parent = list(range(n))
    
    def find(x):
        if parent[x] != x:
            parent[x] = find(parent[x])
        return parent[x]
    
    def union(x, y):
        root_x, root_y = find(x), find(y)
        if root_x != root_y:
            parent[root_x] = root_y
            return True
        return False
    
    components = n
    
    for a, b in edges:
        if union(a, b):
            components -= 1
    
    return components
```

---

## Interval Problems

### 64. Insert Interval
**LeetCode #57** | Medium | ⭐⭐⭐

```python
def insert(intervals: list[list[int]], new_interval: list[int]) -> list[list[int]]:
    """
    Insert interval and merge if needed.
    
    Time: O(n)
    Space: O(n)
    """
    result = []
    i = 0
    n = len(intervals)
    
    # Add intervals before new interval
    while i < n and intervals[i][1] < new_interval[0]:
        result.append(intervals[i])
        i += 1
    
    # Merge overlapping
    while i < n and intervals[i][0] <= new_interval[1]:
        new_interval[0] = min(new_interval[0], intervals[i][0])
        new_interval[1] = max(new_interval[1], intervals[i][1])
        i += 1
    
    result.append(new_interval)
    
    # Add remaining
    while i < n:
        result.append(intervals[i])
        i += 1
    
    return result
```

### 65. Merge Intervals
**LeetCode #56** | Medium | ⭐⭐⭐

```python
def merge(intervals: list[list[int]]) -> list[list[int]]:
    """
    Merge overlapping intervals.
    
    Time: O(n log n)
    Space: O(n)
    """
    intervals.sort(key=lambda x: x[0])
    merged = [intervals[0]]
    
    for current in intervals[1:]:
        last = merged[-1]
        
        if current[0] <= last[1]:
            last[1] = max(last[1], current[1])
        else:
            merged.append(current)
    
    return merged
```

### 66. Non-overlapping Intervals
**LeetCode #435** | Medium | ⭐⭐⭐

```python
def erase_overlap_intervals(intervals: list[list[int]]) -> int:
    """
    Minimum intervals to remove.
    
    Time: O(n log n)
    Space: O(1)
    """
    intervals.sort(key=lambda x: x[1])
    
    count = 0
    end = intervals[0][1]
    
    for i in range(1, len(intervals)):
        if intervals[i][0] < end:
            count += 1
        else:
            end = intervals[i][1]
    
    return count
```

### 67. Meeting Rooms (LeetCode Premium)
**LeetCode #252** | Easy | ⭐⭐

```python
def can_attend_meetings(intervals: list[list[int]]) -> bool:
    """
    Check if person can attend all meetings.
    
    Time: O(n log n)
    Space: O(1)
    """
    intervals.sort(key=lambda x: x[0])
    
    for i in range(1, len(intervals)):
        if intervals[i][0] < intervals[i-1][1]:
            return False
    
    return True
```

### 68. Meeting Rooms II (LeetCode Premium)
**LeetCode #253** | Medium | ⭐⭐⭐

```python
def min_meeting_rooms(intervals: list[list[int]]) -> int:
    """
    Minimum meeting rooms needed.
    
    Time: O(n log n)
    Space: O(n)
    """
    starts = sorted([i[0] for i in intervals])
    ends = sorted([i[1] for i in intervals])
    
    rooms = max_rooms = 0
    s = e = 0
    
    while s < len(intervals):
        if starts[s] < ends[e]:
            rooms += 1
            max_rooms = max(max_rooms, rooms)
            s += 1
        else:
            rooms -= 1
            e += 1
    
    return max_rooms
```

---

## Matrix Problems

### 69. Set Matrix Zeroes
**LeetCode #73** | Medium | ⭐⭐⭐

```python
def set_zeroes(matrix: list[list[int]]) -> None:
    """
    Set entire row and column to 0 if element is 0.
    
    Time: O(m × n)
    Space: O(1)
    """
    m, n = len(matrix), len(matrix[0])
    first_row_zero = any(matrix[0][j] == 0 for j in range(n))
    first_col_zero = any(matrix[i][0] == 0 for i in range(m))
    
    # Use first row and column as markers
    for i in range(1, m):
        for j in range(1, n):
            if matrix[i][j] == 0:
                matrix[i][0] = 0
                matrix[0][j] = 0
    
    # Set zeros
    for i in range(1, m):
        for j in range(1, n):
            if matrix[i][0] == 0 or matrix[0][j] == 0:
                matrix[i][j] = 0
    
    # Handle first row and column
    if first_row_zero:
        for j in range(n):
            matrix[0][j] = 0
    
    if first_col_zero:
        for i in range(m):
            matrix[i][0] = 0
```

### 70. Spiral Matrix
**LeetCode #54** | Medium | ⭐⭐⭐

```python
def spiral_order(matrix: list[list[int]]) -> list[int]:
    """
    Return elements in spiral order.
    
    Time: O(m × n)
    Space: O(1)
    """
    if not matrix:
        return []
    
    result = []
    top, bottom = 0, len(matrix) - 1
    left, right = 0, len(matrix[0]) - 1
    
    while top <= bottom and left <= right:
        # Right
        for col in range(left, right + 1):
            result.append(matrix[top][col])
        top += 1
        
        # Down
        for row in range(top, bottom + 1):
            result.append(matrix[row][right])
        right -= 1
        
        # Left
        if top <= bottom:
            for col in range(right, left - 1, -1):
                result.append(matrix[bottom][col])
            bottom -= 1
        
        # Up
        if left <= right:
            for row in range(bottom, top - 1, -1):
                result.append(matrix[row][left])
            left += 1
    
    return result
```

### 71. Rotate Image
**LeetCode #48** | Medium | ⭐⭐⭐

```python
def rotate(matrix: list[list[int]]) -> None:
    """
    Rotate matrix 90° clockwise in-place.
    
    Time: O(n²)
    Space: O(1)
    """
    n = len(matrix)
    
    # Transpose
    for i in range(n):
        for j in range(i + 1, n):
            matrix[i][j], matrix[j][i] = matrix[j][i], matrix[i][j]
    
    # Reverse rows
    for i in range(n):
        matrix[i].reverse()
```

### 72. Word Search
**LeetCode #79** | Medium | ⭐⭐⭐

```python
def exist(board: list[list[str]], word: str) -> bool:
    """
    Check if word exists in board.
    
    Time: O(m × n × 4^L)
    Space: O(L)
    """
    m, n = len(board), len(board[0])
    
    def dfs(i, j, k):
        if k == len(word):
            return True
        
        if i < 0 or i >= m or j < 0 or j >= n or board[i][j] != word[k]:
            return False
        
        temp = board[i][j]
        board[i][j] = '#'
        
        found = (dfs(i+1, j, k+1) or 
                 dfs(i-1, j, k+1) or 
                 dfs(i, j+1, k+1) or 
                 dfs(i, j-1, k+1))
        
        board[i][j] = temp
        
        return found
    
    for i in range(m):
        for j in range(n):
            if dfs(i, j, 0):
                return True
    
    return False
```

---

## Heap Problems

### 73. Top K Frequent Elements
**LeetCode #347** | Medium | ⭐⭐⭐

```python
def top_k_frequent(nums: list[int], k: int) -> list[int]:
    """
    Find k most frequent elements.
    
    Time: O(n log k)
    Space: O(n)
    """
    from collections import Counter
    import heapq
    
    count = Counter(nums)
    return heapq.nlargest(k, count.keys(), key=count.get)
```

### 74. Find Median from Data Stream
**LeetCode #295** | Hard | ⭐⭐⭐

```python
import heapq

class MedianFinder:
    """
    Find median from data stream.
    
    Time: O(log n) add, O(1) find
    Space: O(n)
    """
    
    def __init__(self):
        self.small = []  # Max heap (negated)
        self.large = []  # Min heap
    
    def addNum(self, num: int) -> None:
        """Add number to data structure."""
        heapq.heappush(self.small, -num)
        
        # Ensure max of small <= min of large
        if self.small and self.large and -self.small[0] > self.large[0]:
            heapq.heappush(self.large, -heapq.heappop(self.small))
        
        # Balance sizes
        if len(self.small) > len(self.large) + 1:
            heapq.heappush(self.large, -heapq.heappop(self.small))
        if len(self.large) > len(self.small):
            heapq.heappush(self.small, -heapq.heappop(self.large))
    
    def findMedian(self) -> float:
        """Find median."""
        if len(self.small) > len(self.large):
            return -self.small[0]
        return (-self.small[0] + self.large[0]) / 2
```

### 75. Alien Dictionary (LeetCode Premium)
**LeetCode #269** | Hard | ⭐⭐⭐

```python
def alien_order(words: list[str]) -> str:
    """
    Derive alien language order using topological sort.
    
    Time: O(total characters)
    Space: O(1) - at most 26 letters
    """
    from collections import defaultdict, deque
    
    # Build graph
    graph = defaultdict(set)
    in_degree = {c: 0 for word in words for c in word}
    
    for i in range(len(words) - 1):
        word1, word2 = words[i], words[i + 1]
        min_len = min(len(word1), len(word2))
        
        # Invalid if word1 is prefix of word2 but longer
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
        char = queue.popleft()
        result.append(char)
        
        for neighbor in graph[char]:
            in_degree[neighbor] -= 1
            if in_degree[neighbor] == 0:
                queue.append(neighbor)
    
    return ''.join(result) if len(result) == len(in_degree) else ""
```

---

## 🎉 Blind 75 Complete!

**All 75 problems solved with:**
- ✅ Clear explanations
- ✅ Time & space complexity
- ✅ Multiple approaches where applicable
- ✅ Clean, production-ready code
- ✅ Organized by topic

### Topics Covered:
1. **Arrays** (10 problems)
2. **Binary** (5 problems)
3. **Dynamic Programming** (11 problems)
4. **Strings** (10 problems)
5. **Linked Lists** (6 problems)
6. **Trees** (14 problems)
7. **Graphs** (7 problems)
8. **Intervals** (5 problems)
9. **Matrix** (4 problems)
10. **Heap** (3 problems)

### Study Tips:
- Start with Easy → Medium → Hard
- Focus on patterns, not memorization
- Practice each problem 2-3 times
- Time yourself on second attempt
- Explain solution out loud

**Happy Coding! 🚀**