# 💎 Dynamic Programming - Python DSA

> Break down problems into overlapping subproblems

---

## 📚 Table of Contents

1. [Introduction](#introduction)
2. [Core Concepts](#core-concepts)
3. [1D DP Problems](#1d-dp-problems)
4. [2D DP Problems](#2d-dp-problems)
5. [String DP](#string-dp)
6. [Advanced Patterns](#advanced-patterns)
7. [Interview Tips](#interview-tips)
8. [Practice Problems](#practice-problems)

---

## Introduction

**Dynamic Programming (DP)** solves complex problems by breaking them into simpler subproblems and storing results.

### Key Characteristics
- ✅ **Overlapping subproblems** - Same subproblems solved multiple times
- ✅ **Optimal substructure** - Optimal solution contains optimal subsolutions
- ✅ **Two approaches** - Top-down (memoization) and bottom-up (tabulation)
- ✅ **Trade space for time** - Store results to avoid recomputation

### When to Use
- Problem has **overlapping subproblems**
- Asks for **optimization** (min/max/count)
- Can break into **smaller similar** problems
- Keywords: "maximum/minimum", "count ways", "longest/shortest"

### DP vs Divide and Conquer
```python
# Divide and Conquer (Merge Sort)
- Subproblems are independent
- No overlapping subproblems
- Example: Merge Sort, Binary Search

# Dynamic Programming (Fibonacci)
- Subproblems overlap
- Store results to reuse
- Example: Fibonacci, Knapsack
```

---

## Core Concepts

### 1. Memoization (Top-Down)

Start with original problem, recursively break down, cache results.

```python
def fib_memo(n: int, memo: dict = None) -> int:
    """
    Fibonacci with memoization.
    
    Time: O(n)
    Space: O(n)
    """
    if memo is None:
        memo = {}
    
    if n in memo:
        return memo[n]
    
    if n <= 1:
        return n
    
    memo[n] = fib_memo(n - 1, memo) + fib_memo(n - 2, memo)
    return memo[n]

# Test
print(fib_memo(10))  # 55
```

### 2. Tabulation (Bottom-Up)

Start from base cases, build up to solution iteratively.

```python
def fib_tab(n: int) -> int:
    """
    Fibonacci with tabulation.
    
    Time: O(n)
    Space: O(n)
    """
    if n <= 1:
        return n
    
    dp = [0] * (n + 1)
    dp[1] = 1
    
    for i in range(2, n + 1):
        dp[i] = dp[i - 1] + dp[i - 2]
    
    return dp[n]

# Test
print(fib_tab(10))  # 55
```

### 3. Space Optimization

Often only need last few values, not entire array.

```python
def fib_optimized(n: int) -> int:
    """
    Fibonacci with O(1) space.
    
    Time: O(n)
    Space: O(1)
    """
    if n <= 1:
        return n
    
    prev2, prev1 = 0, 1
    
    for _ in range(2, n + 1):
        curr = prev1 + prev2
        prev2, prev1 = prev1, curr
    
    return prev1

# Test
print(fib_optimized(10))  # 55
```

---

## 1D DP Problems

### Problem 1: Climbing Stairs

```python
def climb_stairs(n: int) -> int:
    """
    Count ways to climb n stairs (1 or 2 steps at a time).
    
    Time: O(n)
    Space: O(1)
    
    Example: n = 3
    Output: 3 (1+1+1, 1+2, 2+1)
    """
    if n <= 2:
        return n
    
    prev2, prev1 = 1, 2
    
    for _ in range(3, n + 1):
        curr = prev1 + prev2
        prev2, prev1 = prev1, curr
    
    return prev1

# Test
print(climb_stairs(5))  # 8
```

### Problem 2: House Robber

```python
def rob(nums: list[int]) -> int:
    """
    Maximum money from non-adjacent houses.
    
    Time: O(n)
    Space: O(1)
    
    Example: nums = [2,7,9,3,1]
    Output: 12 (2+9+1)
    """
    if not nums:
        return 0
    if len(nums) <= 2:
        return max(nums)
    
    prev2, prev1 = nums[0], max(nums[0], nums[1])
    
    for i in range(2, len(nums)):
        # Either rob current + prev2 or skip current
        curr = max(nums[i] + prev2, prev1)
        prev2, prev1 = prev1, curr
    
    return prev1

# Test
print(rob([2,7,9,3,1]))  # 12
```

### Problem 3: House Robber II (Circular)

```python
def rob_circular(nums: list[int]) -> int:
    """
    Maximum money from circular arrangement.
    
    Time: O(n)
    Space: O(1)
    
    Example: nums = [2,3,2]
    Output: 3 (can't rob both first and last)
    """
    def rob_linear(houses):
        prev2, prev1 = 0, 0
        for money in houses:
            curr = max(money + prev2, prev1)
            prev2, prev1 = prev1, curr
        return prev1
    
    if len(nums) == 1:
        return nums[0]
    
    # Either skip first or skip last
    return max(rob_linear(nums[:-1]), rob_linear(nums[1:]))

# Test
print(rob_circular([2,3,2]))  # 3
```

### Problem 4: Coin Change

```python
def coin_change(coins: list[int], amount: int) -> int:
    """
    Minimum coins to make amount.
    
    Time: O(amount * len(coins))
    Space: O(amount)
    
    Example: coins = [1,2,5], amount = 11
    Output: 3 (5+5+1)
    """
    dp = [float('inf')] * (amount + 1)
    dp[0] = 0
    
    for i in range(1, amount + 1):
        for coin in coins:
            if coin <= i:
                dp[i] = min(dp[i], dp[i - coin] + 1)
    
    return dp[amount] if dp[amount] != float('inf') else -1

# Test
print(coin_change([1,2,5], 11))  # 3
```

### Problem 5: Coin Change 2 (Count Ways)

```python
def change(amount: int, coins: list[int]) -> int:
    """
    Number of ways to make amount.
    
    Time: O(amount * len(coins))
    Space: O(amount)
    
    Example: amount = 5, coins = [1,2,5]
    Output: 4 (5, 2+2+1, 2+1+1+1, 1+1+1+1+1)
    """
    dp = [0] * (amount + 1)
    dp[0] = 1
    
    # For each coin
    for coin in coins:
        # Update all amounts that can include this coin
        for i in range(coin, amount + 1):
            dp[i] += dp[i - coin]
    
    return dp[amount]

# Test
print(change(5, [1,2,5]))  # 4
```

### Problem 6: Longest Increasing Subsequence (LIS)

```python
def length_of_lis(nums: list[int]) -> int:
    """
    Length of longest increasing subsequence.
    
    Time: O(n²) - DP solution
    Space: O(n)
    
    Example: nums = [10,9,2,5,3,7,101,18]
    Output: 4 ([2,3,7,101])
    """
    if not nums:
        return 0
    
    n = len(nums)
    dp = [1] * n  # Each element is LIS of length 1
    
    for i in range(1, n):
        for j in range(i):
            if nums[j] < nums[i]:
                dp[i] = max(dp[i], dp[j] + 1)
    
    return max(dp)

# Test
print(length_of_lis([10,9,2,5,3,7,101,18]))  # 4
```

### Problem 7: LIS (Binary Search Optimization)

```python
import bisect

def length_of_lis_optimized(nums: list[int]) -> int:
    """
    LIS using binary search.
    
    Time: O(n log n)
    Space: O(n)
    """
    sub = []
    
    for num in nums:
        pos = bisect.bisect_left(sub, num)
        
        if pos == len(sub):
            sub.append(num)
        else:
            sub[pos] = num
    
    return len(sub)

# Test
print(length_of_lis_optimized([10,9,2,5,3,7,101,18]))  # 4
```

### Problem 8: Maximum Subarray (Kadane's Algorithm)

```python
def max_subarray(nums: list[int]) -> int:
    """
    Maximum sum of contiguous subarray.
    
    Time: O(n)
    Space: O(1)
    
    Example: nums = [-2,1,-3,4,-1,2,1,-5,4]
    Output: 6 ([4,-1,2,1])
    """
    max_sum = curr_sum = nums[0]
    
    for num in nums[1:]:
        # Either extend current subarray or start new
        curr_sum = max(num, curr_sum + num)
        max_sum = max(max_sum, curr_sum)
    
    return max_sum

# Test
print(max_subarray([-2,1,-3,4,-1,2,1,-5,4]))  # 6
```

### Problem 9: Maximum Product Subarray

```python
def max_product(nums: list[int]) -> int:
    """
    Maximum product of contiguous subarray.
    
    Time: O(n)
    Space: O(1)
    
    Example: nums = [2,3,-2,4]
    Output: 6 ([2,3])
    """
    max_prod = min_prod = result = nums[0]
    
    for num in nums[1:]:
        # Negative number swaps max and min
        if num < 0:
            max_prod, min_prod = min_prod, max_prod
        
        max_prod = max(num, max_prod * num)
        min_prod = min(num, min_prod * num)
        
        result = max(result, max_prod)
    
    return result

# Test
print(max_product([2,3,-2,4]))  # 6
```

### Problem 10: Decode Ways

```python
def num_decodings(s: str) -> int:
    """
    Number of ways to decode string.
    
    Time: O(n)
    Space: O(1)
    
    Example: s = "226"
    Output: 3 ("2,2,6", "22,6", "2,26")
    """
    if not s or s[0] == '0':
        return 0
    
    prev2, prev1 = 1, 1
    
    for i in range(1, len(s)):
        curr = 0
        
        # Single digit
        if s[i] != '0':
            curr += prev1
        
        # Two digits
        two_digit = int(s[i-1:i+1])
        if 10 <= two_digit <= 26:
            curr += prev2
        
        prev2, prev1 = prev1, curr
    
    return prev1

# Test
print(num_decodings("226"))  # 3
print(num_decodings("12"))   # 2
```

---

## 2D DP Problems

### Problem 11: Unique Paths

```python
def unique_paths(m: int, n: int) -> int:
    """
    Count paths from top-left to bottom-right.
    
    Time: O(m * n)
    Space: O(n) - space optimized
    
    Example: m = 3, n = 7
    Output: 28
    """
    dp = [1] * n
    
    for _ in range(1, m):
        for j in range(1, n):
            dp[j] += dp[j - 1]
    
    return dp[-1]

# Test
print(unique_paths(3, 7))  # 28
```

### Problem 12: Unique Paths II (Obstacles)

```python
def unique_paths_with_obstacles(grid: list[list[int]]) -> int:
    """
    Count paths with obstacles.
    
    Time: O(m * n)
    Space: O(n)
    
    Example: grid = [[0,0,0],[0,1,0],[0,0,0]]
    Output: 2
    """
    if not grid or grid[0][0] == 1:
        return 0
    
    m, n = len(grid), len(grid[0])
    dp = [0] * n
    dp[0] = 1
    
    for i in range(m):
        for j in range(n):
            if grid[i][j] == 1:
                dp[j] = 0
            elif j > 0:
                dp[j] += dp[j - 1]
    
    return dp[-1]

# Test
grid = [[0,0,0],[0,1,0],[0,0,0]]
print(unique_paths_with_obstacles(grid))  # 2
```

### Problem 13: Minimum Path Sum

```python
def min_path_sum(grid: list[list[int]]) -> int:
    """
    Minimum sum path from top-left to bottom-right.
    
    Time: O(m * n)
    Space: O(n)
    
    Example: grid = [[1,3,1],[1,5,1],[4,2,1]]
    Output: 7 (1→3→1→1→1)
    """
    m, n = len(grid), len(grid[0])
    dp = [float('inf')] * n
    dp[0] = 0
    
    for i in range(m):
        for j in range(n):
            if j == 0:
                dp[j] = dp[j] + grid[i][j]
            else:
                dp[j] = min(dp[j], dp[j - 1]) + grid[i][j]
    
    return dp[-1]

# Test
grid = [[1,3,1],[1,5,1],[4,2,1]]
print(min_path_sum(grid))  # 7
```

### Problem 14: Longest Common Subsequence (LCS)

```python
def longest_common_subsequence(text1: str, text2: str) -> int:
    """
    Length of longest common subsequence.
    
    Time: O(m * n)
    Space: O(min(m, n))
    
    Example: text1 = "abcde", text2 = "ace"
    Output: 3 ("ace")
    """
    if len(text1) < len(text2):
        text1, text2 = text2, text1
    
    m, n = len(text1), len(text2)
    dp = [0] * (n + 1)
    
    for i in range(1, m + 1):
        prev = 0
        for j in range(1, n + 1):
            temp = dp[j]
            if text1[i - 1] == text2[j - 1]:
                dp[j] = prev + 1
            else:
                dp[j] = max(dp[j], dp[j - 1])
            prev = temp
    
    return dp[n]

# Test
print(longest_common_subsequence("abcde", "ace"))  # 3
```

### Problem 15: Edit Distance

```python
def min_distance(word1: str, word2: str) -> int:
    """
    Minimum operations to convert word1 to word2.
    
    Time: O(m * n)
    Space: O(min(m, n))
    
    Example: word1 = "horse", word2 = "ros"
    Output: 3 (horse→rorse→rose→ros)
    """
    m, n = len(word1), len(word2)
    
    if m < n:
        word1, word2 = word2, word1
        m, n = n, m
    
    dp = list(range(n + 1))
    
    for i in range(1, m + 1):
        prev = dp[0]
        dp[0] = i
        
        for j in range(1, n + 1):
            temp = dp[j]
            
            if word1[i - 1] == word2[j - 1]:
                dp[j] = prev
            else:
                dp[j] = 1 + min(prev, dp[j], dp[j - 1])
                # prev = replace, dp[j] = delete, dp[j-1] = insert
            
            prev = temp
    
    return dp[n]

# Test
print(min_distance("horse", "ros"))  # 3
```

### Problem 16: 0/1 Knapsack

```python
def knapsack(weights: list[int], values: list[int], capacity: int) -> int:
    """
    Maximum value with capacity constraint.
    
    Time: O(n * capacity)
    Space: O(capacity)
    
    Example: weights = [1,3,4,5], values = [1,4,5,7], capacity = 7
    Output: 9 (items 2 and 3)
    """
    n = len(weights)
    dp = [0] * (capacity + 1)
    
    for i in range(n):
        # Traverse backwards to avoid using same item twice
        for w in range(capacity, weights[i] - 1, -1):
            dp[w] = max(dp[w], dp[w - weights[i]] + values[i])
    
    return dp[capacity]

# Test
print(knapsack([1,3,4,5], [1,4,5,7], 7))  # 9
```

---

## String DP

### Problem 17: Longest Palindromic Substring

```python
def longest_palindrome(s: str) -> str:
    """
    Find longest palindromic substring.
    
    Time: O(n²)
    Space: O(1)
    
    Example: s = "babad"
    Output: "bab" or "aba"
    """
    def expand_around_center(left, right):
        while left >= 0 and right < len(s) and s[left] == s[right]:
            left -= 1
            right += 1
        return right - left - 1
    
    if not s:
        return ""
    
    start = end = 0
    
    for i in range(len(s)):
        len1 = expand_around_center(i, i)      # Odd length
        len2 = expand_around_center(i, i + 1)  # Even length
        max_len = max(len1, len2)
        
        if max_len > end - start:
            start = i - (max_len - 1) // 2
            end = i + max_len // 2
    
    return s[start:end + 1]

# Test
print(longest_palindrome("babad"))  # "bab" or "aba"
```

### Problem 18: Palindromic Substrings

```python
def count_substrings(s: str) -> int:
    """
    Count all palindromic substrings.
    
    Time: O(n²)
    Space: O(1)
    
    Example: s = "abc"
    Output: 3 ("a", "b", "c")
    """
    def expand_around_center(left, right):
        count = 0
        while left >= 0 and right < len(s) and s[left] == s[right]:
            count += 1
            left -= 1
            right += 1
        return count
    
    result = 0
    for i in range(len(s)):
        result += expand_around_center(i, i)      # Odd
        result += expand_around_center(i, i + 1)  # Even
    
    return result

# Test
print(count_substrings("abc"))  # 3
print(count_substrings("aaa"))  # 6
```

### Problem 19: Word Break

```python
def word_break(s: str, word_dict: list[str]) -> bool:
    """
    Check if string can be segmented into words.
    
    Time: O(n² * m) where m is max word length
    Space: O(n)
    
    Example: s = "leetcode", wordDict = ["leet","code"]
    Output: True
    """
    word_set = set(word_dict)
    n = len(s)
    dp = [False] * (n + 1)
    dp[0] = True
    
    for i in range(1, n + 1):
        for j in range(i):
            if dp[j] and s[j:i] in word_set:
                dp[i] = True
                break
    
    return dp[n]

# Test
print(word_break("leetcode", ["leet", "code"]))  # True
```

### Problem 20: Word Break II

```python
def word_break_ii(s: str, word_dict: list[str]) -> list[str]:
    """
    Return all possible word break sentences.
    
    Time: O(2^n) worst case
    Space: O(2^n)
    
    Example: s = "catsanddog", wordDict = ["cat","cats","and","sand","dog"]
    Output: ["cats and dog","cat sand dog"]
    """
    word_set = set(word_dict)
    memo = {}
    
    def backtrack(start):
        if start in memo:
            return memo[start]
        
        if start == len(s):
            return [""]
        
        sentences = []
        for end in range(start + 1, len(s) + 1):
            word = s[start:end]
            if word in word_set:
                for suffix in backtrack(end):
                    sentences.append(word + (" " + suffix if suffix else ""))
        
        memo[start] = sentences
        return sentences
    
    return backtrack(0)

# Test
print(word_break_ii("catsanddog", ["cat","cats","and","sand","dog"]))
```

---

## Advanced Patterns

### Problem 21: Partition Equal Subset Sum

```python
def can_partition(nums: list[int]) -> bool:
    """
    Check if array can be partitioned into equal sum subsets.
    
    Time: O(n * sum)
    Space: O(sum)
    
    Example: nums = [1,5,11,5]
    Output: True ([1,5,5] and [11])
    """
    total = sum(nums)
    if total % 2:
        return False
    
    target = total // 2
    dp = [False] * (target + 1)
    dp[0] = True
    
    for num in nums:
        for i in range(target, num - 1, -1):
            dp[i] = dp[i] or dp[i - num]
    
    return dp[target]

# Test
print(can_partition([1,5,11,5]))  # True
```

### Problem 22: Target Sum

```python
def find_target_sum_ways(nums: list[int], target: int) -> int:
    """
    Count ways to add +/- to reach target.
    
    Time: O(n * sum)
    Space: O(sum)
    
    Example: nums = [1,1,1,1,1], target = 3
    Output: 5
    """
    from collections import defaultdict
    
    dp = defaultdict(int)
    dp[0] = 1
    
    for num in nums:
        next_dp = defaultdict(int)
        for curr_sum, count in dp.items():
            next_dp[curr_sum + num] += count
            next_dp[curr_sum - num] += count
        dp = next_dp
    
    return dp[target]

# Test
print(find_target_sum_ways([1,1,1,1,1], 3))  # 5
```

### Problem 23: Burst Balloons

```python
def max_coins(nums: list[int]) -> int:
    """
    Maximum coins from bursting balloons.
    
    Time: O(n³)
    Space: O(n²)
    
    Example: nums = [3,1,5,8]
    Output: 167
    """
    nums = [1] + nums + [1]
    n = len(nums)
    dp = [[0] * n for _ in range(n)]
    
    for length in range(2, n):
        for left in range(n - length):
            right = left + length
            
            for i in range(left + 1, right):
                coins = nums[left] * nums[i] * nums[right]
                coins += dp[left][i] + dp[i][right]
                dp[left][right] = max(dp[left][right], coins)
    
    return dp[0][n - 1]

# Test
print(max_coins([3,1,5,8]))  # 167
```

---

## Interview Tips

### 1. Identify DP Problems

```python
# Signs of DP:
✅ "Maximum/minimum"
✅ "Count ways"
✅ "Longest/shortest"
✅ "Can partition/subset"
✅ Overlapping subproblems
✅ Optimal substructure
```

### 2. DP Problem-Solving Steps

```python
# 1. Define state
dp[i] = "what does this represent?"

# 2. Find recurrence relation
dp[i] = f(dp[i-1], dp[i-2], ...)

# 3. Initialize base cases
dp[0] = ...

# 4. Determine iteration order
# Usually left to right, top to bottom

# 5. Space optimization
# Can we use O(1) or O(n) instead of O(n²)?
```

### 3. Common DP Patterns

```python
# 1. Linear DP (1D)
dp[i] = max(dp[i-1] + nums[i], nums[i])

# 2. Grid DP (2D)
dp[i][j] = dp[i-1][j] + dp[i][j-1]

# 3. String DP
if s[i] == s[j]:
    dp[i][j] = dp[i+1][j-1]

# 4. Subset/Knapsack
for item in items:
    for capacity in range(max_cap, item_weight - 1, -1):
        dp[capacity] = max(dp[capacity], dp[capacity - item_weight] + value)
```

### 4. Memoization vs Tabulation

```python
# Memoization (Top-Down):
✅ Easier to implement
✅ Only solves needed subproblems
❌ Recursion overhead
❌ Stack space

# Tabulation (Bottom-Up):
✅ No recursion overhead
✅ Better space optimization
✅ Iterative (no stack overflow)
❌ Solves all subproblems
```

---

## Practice Problems

### Easy
1. [Climbing Stairs](https://leetcode.com/problems/climbing-stairs/)
2. [Min Cost Climbing Stairs](https://leetcode.com/problems/min-cost-climbing-stairs/)
3. [Divisor Game](https://leetcode.com/problems/divisor-game/)

### Medium
1. [House Robber](https://leetcode.com/problems/house-robber/)
2. [House Robber II](https://leetcode.com/problems/house-robber-ii/)
3. [Coin Change](https://leetcode.com/problems/coin-change/)
4. [Coin Change 2](https://leetcode.com/problems/coin-change-2/)
5. [Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/)
6. [Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)
7. [Maximum Product Subarray](https://leetcode.com/problems/maximum-product-subarray/)
8. [Decode Ways](https://leetcode.com/problems/decode-ways/)
9. [Unique Paths](https://leetcode.com/problems/unique-paths/)
10. [Unique Paths II](https://leetcode.com/problems/unique-paths-ii/)
11. [Minimum Path Sum](https://leetcode.com/problems/minimum-path-sum/)
12. [Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence/)
13. [Edit Distance](https://leetcode.com/problems/edit-distance/)
14. [Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring/)
15. [Palindromic Substrings](https://leetcode.com/problems/palindromic-substrings/)
16. [Word Break](https://leetcode.com/problems/word-break/)
17. [Partition Equal Subset Sum](https://leetcode.com/problems/partition-equal-subset-sum/)
18. [Target Sum](https://leetcode.com/problems/target-sum/)

### Hard
1. [Word Break II](https://leetcode.com/problems/word-break-ii/)
2. [Burst Balloons](https://leetcode.com/problems/burst-balloons/)
3. [Regular Expression Matching](https://leetcode.com/problems/regular-expression-matching/)
4. [Wildcard Matching](https://leetcode.com/problems/wildcard-matching/)
5. [Distinct Subsequences](https://leetcode.com/problems/distinct-subsequences/)

---

## Summary

### Key Takeaways
- ✅ **Overlapping subproblems** - Key indicator
- ✅ **Two approaches** - Memoization (top-down) and tabulation (bottom-up)
- ✅ **Space optimization** - Often possible
- ✅ **Most important pattern** - Master this for interviews!

### Quick Reference

```python
# 1D DP Template
dp = [0] * (n + 1)
dp[0] = base_case
for i in range(1, n + 1):
    dp[i] = f(dp[i-1], dp[i-2], ...)
return dp[n]

# 2D DP Template
dp = [[0] * (n + 1) for _ in range(m + 1)]
for i in range(1, m + 1):
    for j in range(1, n + 1):
        dp[i][j] = f(dp[i-1][j], dp[i][j-1], ...)
return dp[m][n]

# Knapsack Template
dp = [0] * (capacity + 1)
for item in items:
    for w in range(capacity, weight[item] - 1, -1):
        dp[w] = max(dp[w], dp[w - weight[item]] + value[item])
return dp[capacity]
```

---

**Next**: [Backtracking →](../04-backtracking/README.md)

**Happy Coding! 🚀**
