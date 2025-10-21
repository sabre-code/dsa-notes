# 💰 Greedy Algorithms - Python DSA

> Make locally optimal choices for global solutions

---

## 📚 Table of Contents

1. [Introduction](#introduction)
2. [Core Concepts](#core-concepts)
3. [Classic Problems](#classic-problems)
4. [Interval Problems](#interval-problems)
5. [Array Problems](#array-problems)
6. [Interview Tips](#interview-tips)
7. [Practice Problems](#practice-problems)

---

## Introduction

**Greedy Algorithms** make locally optimal choices at each step, hoping to find global optimum.

### Key Characteristics
- ✅ **Local optimum** - Choose best option at each step
- ✅ **No backtracking** - Decisions are final
- ✅ **Fast** - Usually O(n) or O(n log n)
- ✅ **Not always correct** - Must prove greedy choice property

### When to Use
- Problem has **greedy choice property**
- Problem has **optimal substructure**
- Can **prove** greedy works
- Keywords: "maximum/minimum", "earliest", "latest"

### Greedy vs Dynamic Programming
```python
# Greedy:
- Make choice based on current state
- No looking back
- Usually faster
- Not always optimal

# Dynamic Programming:
- Consider all possibilities
- Build solution from subproblems
- Always optimal (if correct)
- Usually slower
```

---

## Core Concepts

### Greedy Choice Property

The problem must satisfy: **locally optimal choice leads to globally optimal solution**.

```python
# Example: Coin Change (Greedy Works for US coins)
def coin_change_greedy(amount, coins=[25, 10, 5, 1]):
    """
    Greedy works for US coins.
    
    Time: O(n)
    Space: O(1)
    """
    coins.sort(reverse=True)
    count = 0
    
    for coin in coins:
        count += amount // coin
        amount %= coin
    
    return count

# Test
print(coin_change_greedy(41))  # 5 (25 + 10 + 5 + 1)

# ⚠️ Greedy FAILS for coins = [1, 3, 4], amount = 6
# Greedy: 4 + 1 + 1 = 3 coins
# Optimal: 3 + 3 = 2 coins
# Need DP for arbitrary coins!
```

### Proving Greedy Correctness

```python
# To prove greedy algorithm is correct:
1. Greedy Choice Property:
   - Show that locally optimal choice is part of global optimum
   
2. Optimal Substructure:
   - Show that optimal solution contains optimal subsolutions
   
3. Exchange Argument:
   - Show that any optimal solution can be transformed to greedy solution
```

---

## Classic Problems

### Problem 1: Activity Selection

```python
def activity_selection(start: list[int], finish: list[int]) -> int:
    """
    Maximum non-overlapping activities.
    
    Time: O(n log n)
    Space: O(n)
    
    Example: start = [1,3,0,5,8,5], finish = [2,4,6,7,9,9]
    Output: 4 (activities 0,1,3,4)
    """
    # Sort by finish time
    activities = sorted(zip(start, finish), key=lambda x: x[1])
    
    count = 1
    last_finish = activities[0][1]
    
    for s, f in activities[1:]:
        if s >= last_finish:  # Non-overlapping
            count += 1
            last_finish = f
    
    return count

# Test
print(activity_selection([1,3,0,5,8,5], [2,4,6,7,9,9]))  # 4
```

### Problem 2: Jump Game

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
    
    for i in range(len(nums)):
        if i > max_reach:
            return False
        
        max_reach = max(max_reach, i + nums[i])
        
        if max_reach >= len(nums) - 1:
            return True
    
    return True

# Test
print(can_jump([2,3,1,1,4]))  # True
print(can_jump([3,2,1,0,4]))  # False
```

### Problem 3: Jump Game II

```python
def jump(nums: list[int]) -> int:
    """
    Minimum jumps to reach last index.
    
    Time: O(n)
    Space: O(1)
    
    Example: nums = [2,3,1,1,4]
    Output: 2 (jump 1 step to index 1, then 3 steps to last)
    """
    jumps = 0
    current_end = 0
    farthest = 0
    
    for i in range(len(nums) - 1):
        farthest = max(farthest, i + nums[i])
        
        if i == current_end:
            jumps += 1
            current_end = farthest
    
    return jumps

# Test
print(jump([2,3,1,1,4]))  # 2
```

### Problem 4: Gas Station

```python
def can_complete_circuit(gas: list[int], cost: list[int]) -> int:
    """
    Find starting gas station to complete circuit.
    
    Time: O(n)
    Space: O(1)
    
    Example: gas = [1,2,3,4,5], cost = [3,4,5,1,2]
    Output: 3
    """
    if sum(gas) < sum(cost):
        return -1
    
    start = 0
    tank = 0
    
    for i in range(len(gas)):
        tank += gas[i] - cost[i]
        
        if tank < 0:
            start = i + 1
            tank = 0
    
    return start

# Test
print(can_complete_circuit([1,2,3,4,5], [3,4,5,1,2]))  # 3
```

### Problem 5: Container With Most Water

```python
def max_area(height: list[int]) -> int:
    """
    Maximum water container area.
    
    Time: O(n)
    Space: O(1)
    
    Example: height = [1,8,6,2,5,4,8,3,7]
    Output: 49
    """
    left, right = 0, len(height) - 1
    max_water = 0
    
    while left < right:
        width = right - left
        h = min(height[left], height[right])
        max_water = max(max_water, width * h)
        
        # Move pointer with smaller height
        if height[left] < height[right]:
            left += 1
        else:
            right -= 1
    
    return max_water

# Test
print(max_area([1,8,6,2,5,4,8,3,7]))  # 49
```

---

## Interval Problems

### Problem 6: Merge Intervals

```python
def merge(intervals: list[list[int]]) -> list[list[int]]:
    """
    Merge overlapping intervals.
    
    Time: O(n log n)
    Space: O(n)
    
    Example: intervals = [[1,3],[2,6],[8,10],[15,18]]
    Output: [[1,6],[8,10],[15,18]]
    """
    if not intervals:
        return []
    
    intervals.sort(key=lambda x: x[0])
    merged = [intervals[0]]
    
    for current in intervals[1:]:
        last = merged[-1]
        
        if current[0] <= last[1]:  # Overlapping
            last[1] = max(last[1], current[1])
        else:
            merged.append(current)
    
    return merged

# Test
print(merge([[1,3],[2,6],[8,10],[15,18]]))
# [[1,6],[8,10],[15,18]]
```

### Problem 7: Insert Interval

```python
def insert(intervals: list[list[int]], new_interval: list[int]) -> list[list[int]]:
    """
    Insert and merge interval.
    
    Time: O(n)
    Space: O(n)
    
    Example: intervals = [[1,3],[6,9]], newInterval = [2,5]
    Output: [[1,5],[6,9]]
    """
    result = []
    i = 0
    n = len(intervals)
    
    # Add all intervals before new interval
    while i < n and intervals[i][1] < new_interval[0]:
        result.append(intervals[i])
        i += 1
    
    # Merge overlapping intervals
    while i < n and intervals[i][0] <= new_interval[1]:
        new_interval[0] = min(new_interval[0], intervals[i][0])
        new_interval[1] = max(new_interval[1], intervals[i][1])
        i += 1
    
    result.append(new_interval)
    
    # Add remaining intervals
    while i < n:
        result.append(intervals[i])
        i += 1
    
    return result

# Test
print(insert([[1,3],[6,9]], [2,5]))  # [[1,5],[6,9]]
```

### Problem 8: Non-overlapping Intervals

```python
def erase_overlap_intervals(intervals: list[list[int]]) -> int:
    """
    Minimum intervals to remove to make non-overlapping.
    
    Time: O(n log n)
    Space: O(1)
    
    Example: intervals = [[1,2],[2,3],[3,4],[1,3]]
    Output: 1 (remove [1,3])
    """
    if not intervals:
        return 0
    
    intervals.sort(key=lambda x: x[1])  # Sort by end time
    
    count = 0
    end = intervals[0][1]
    
    for i in range(1, len(intervals)):
        if intervals[i][0] < end:  # Overlapping
            count += 1
        else:
            end = intervals[i][1]
    
    return count

# Test
print(erase_overlap_intervals([[1,2],[2,3],[3,4],[1,3]]))  # 1
```

### Problem 9: Meeting Rooms II

```python
def min_meeting_rooms(intervals: list[list[int]]) -> int:
    """
    Minimum meeting rooms needed.
    
    Time: O(n log n)
    Space: O(n)
    
    Example: intervals = [[0,30],[5,10],[15,20]]
    Output: 2
    """
    if not intervals:
        return 0
    
    starts = sorted([i[0] for i in intervals])
    ends = sorted([i[1] for i in intervals])
    
    rooms = 0
    max_rooms = 0
    s, e = 0, 0
    
    while s < len(starts):
        if starts[s] < ends[e]:
            rooms += 1
            max_rooms = max(max_rooms, rooms)
            s += 1
        else:
            rooms -= 1
            e += 1
    
    return max_rooms

# Test
print(min_meeting_rooms([[0,30],[5,10],[15,20]]))  # 2
```

---

## Array Problems

### Problem 10: Best Time to Buy and Sell Stock

```python
def max_profit(prices: list[int]) -> int:
    """
    Maximum profit from one transaction.
    
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

# Test
print(max_profit([7,1,5,3,6,4]))  # 5
```

### Problem 11: Best Time to Buy and Sell Stock II

```python
def max_profit_multiple(prices: list[int]) -> int:
    """
    Maximum profit from multiple transactions.
    
    Time: O(n)
    Space: O(1)
    
    Example: prices = [7,1,5,3,6,4]
    Output: 7 (buy at 1, sell at 5, buy at 3, sell at 6)
    """
    profit = 0
    
    for i in range(1, len(prices)):
        if prices[i] > prices[i - 1]:
            profit += prices[i] - prices[i - 1]
    
    return profit

# Test
print(max_profit_multiple([7,1,5,3,6,4]))  # 7
```

### Problem 12: Partition Labels

```python
def partition_labels(s: str) -> list[int]:
    """
    Partition string into max parts where each letter in one part.
    
    Time: O(n)
    Space: O(1)
    
    Example: s = "ababcbacadefegdehijhklij"
    Output: [9,7,8]
    """
    # Find last occurrence of each character
    last = {c: i for i, c in enumerate(s)}
    
    result = []
    start = 0
    end = 0
    
    for i, c in enumerate(s):
        end = max(end, last[c])
        
        if i == end:
            result.append(end - start + 1)
            start = i + 1
    
    return result

# Test
print(partition_labels("ababcbacadefegdehijhklij"))
# [9, 7, 8]
```

### Problem 13: Remove K Digits

```python
def remove_k_digits(num: str, k: int) -> str:
    """
    Remove k digits to get smallest number.
    
    Time: O(n)
    Space: O(n)
    
    Example: num = "1432219", k = 3
    Output: "1219"
    """
    stack = []
    
    for digit in num:
        while k > 0 and stack and stack[-1] > digit:
            stack.pop()
            k -= 1
        stack.append(digit)
    
    # Remove remaining k digits from end
    if k > 0:
        stack = stack[:-k]
    
    # Remove leading zeros
    result = ''.join(stack).lstrip('0')
    
    return result if result else '0'

# Test
print(remove_k_digits("1432219", 3))  # "1219"
```

### Problem 14: Task Scheduler

```python
def least_interval(tasks: list[str], n: int) -> int:
    """
    Minimum intervals to complete tasks with cooldown.
    
    Time: O(m) where m is total intervals
    Space: O(1)
    
    Example: tasks = ["A","A","A","B","B","B"], n = 2
    Output: 8 (A -> B -> idle -> A -> B -> idle -> A -> B)
    """
    from collections import Counter
    
    freq = Counter(tasks)
    max_freq = max(freq.values())
    max_count = sum(1 for f in freq.values() if f == max_freq)
    
    # Formula: (max_freq - 1) * (n + 1) + max_count
    intervals = (max_freq - 1) * (n + 1) + max_count
    
    return max(intervals, len(tasks))

# Test
print(least_interval(["A","A","A","B","B","B"], 2))  # 8
```

### Problem 15: Queue Reconstruction by Height

```python
def reconstruct_queue(people: list[list[int]]) -> list[list[int]]:
    """
    Reconstruct queue by height and position.
    
    Time: O(n²)
    Space: O(n)
    
    Example: people = [[7,0],[4,4],[7,1],[5,0],[6,1],[5,2]]
    Output: [[5,0],[7,0],[5,2],[6,1],[4,4],[7,1]]
    """
    # Sort by height desc, then by k asc
    people.sort(key=lambda x: (-x[0], x[1]))
    
    result = []
    for person in people:
        result.insert(person[1], person)
    
    return result

# Test
people = [[7,0],[4,4],[7,1],[5,0],[6,1],[5,2]]
print(reconstruct_queue(people))
```

---

## Interview Tips

### 1. Recognizing Greedy Problems

```python
# Signs of greedy:
✅ "Maximum/minimum" with constraints
✅ Interval scheduling problems
✅ "Earliest/latest" decisions
✅ Can make choice without looking back
✅ Local optimum leads to global optimum

# Common greedy strategies:
- Sort first (by start, end, value, etc.)
- Take largest/smallest available
- Make decision at each step
```

### 2. Proving Greedy Correctness

```python
# Exchange Argument:
1. Assume optimal solution differs from greedy
2. Show you can exchange choices to match greedy
3. Prove exchange doesn't make solution worse
4. Conclude greedy is optimal

# Example: Activity Selection
- Greedy: Choose earliest finishing activity
- Proof: Any optimal solution can be transformed
  to start with earliest finishing activity
```

### 3. Common Greedy Patterns

```python
# Pattern 1: Sort and Process
intervals.sort(key=lambda x: x[1])  # Sort by end
for interval in intervals:
    # Process in order

# Pattern 2: Two Pointers
left, right = 0, len(arr) - 1
while left < right:
    # Make greedy choice
    # Move pointer

# Pattern 3: Min/Max at Each Step
for item in items:
    current = min/max(options)
    # Update state

# Pattern 4: Stack for Monotonic Sequence
stack = []
for item in items:
    while stack and condition:
        stack.pop()
    stack.append(item)
```

### 4. When Greedy Fails

```python
# Greedy DOESN'T work for:
❌ Coin change with arbitrary denominations
❌ 0/1 Knapsack (need DP)
❌ Longest path in graph
❌ Problems needing all solutions

# Use DP instead when:
- Overlapping subproblems
- Need optimal solution guaranteed
- Greedy counterexample exists
```

---

## Practice Problems

### Easy
1. [Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)
2. [Assign Cookies](https://leetcode.com/problems/assign-cookies/)
3. [Lemonade Change](https://leetcode.com/problems/lemonade-change/)

### Medium
1. [Jump Game](https://leetcode.com/problems/jump-game/)
2. [Jump Game II](https://leetcode.com/problems/jump-game-ii/)
3. [Gas Station](https://leetcode.com/problems/gas-station/)
4. [Container With Most Water](https://leetcode.com/problems/container-with-most-water/)
5. [Merge Intervals](https://leetcode.com/problems/merge-intervals/)
6. [Insert Interval](https://leetcode.com/problems/insert-interval/)
7. [Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/)
8. [Meeting Rooms II](https://leetcode.com/problems/meeting-rooms-ii/)
9. [Best Time to Buy and Sell Stock II](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)
10. [Partition Labels](https://leetcode.com/problems/partition-labels/)
11. [Task Scheduler](https://leetcode.com/problems/task-scheduler/)
12. [Queue Reconstruction by Height](https://leetcode.com/problems/queue-reconstruction-by-height/)

### Hard
1. [Remove K Digits](https://leetcode.com/problems/remove-k-digits/)
2. [Candy](https://leetcode.com/problems/candy/)
3. [Minimum Number of Arrows to Burst Balloons](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/)

---

## Summary

### Key Takeaways
- ✅ **Local optimum** - Best choice at each step
- ✅ **Must prove** - Not all greedy algorithms work
- ✅ **Usually fast** - O(n) or O(n log n)
- ✅ **Sort first** - Common strategy

### Quick Reference

```python
# Greedy Template
def greedy_solution(items):
    # 1. Sort if needed
    items.sort(key=some_criterion)
    
    # 2. Initialize result
    result = initial_value
    
    # 3. Make greedy choice at each step
    for item in items:
        if is_valid_choice(item):
            result = update(result, item)
    
    return result

# Interval Template
intervals.sort(key=lambda x: x[1])  # Sort by end
count = 1
end = intervals[0][1]
for start, finish in intervals[1:]:
    if start >= end:
        count += 1
        end = finish
```

---

**Next**: [Two Pointers (Advanced) →](../07-two-pointers-advanced/README.md)

**Happy Coding! 🚀**
