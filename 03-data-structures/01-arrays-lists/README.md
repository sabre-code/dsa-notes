# 📊 Arrays & Lists - Python

> Master one of the most fundamental data structures in programming

**Why Arrays/Lists Matter**: Arrays are the foundation of nearly every data structure and algorithm. About 30-40% of interview problems directly involve arrays or use array techniques. Understanding arrays deeply—not just how to use them, but how they work internally—is crucial for writing efficient code and solving complex problems.

**What You'll Learn**: In this chapter, you'll understand how Python lists differ from traditional arrays, master common array manipulation patterns (two pointers, sliding window, prefix sums), learn when arrays are the right choice vs other data structures, and solve essential array problems that appear in every technical interview.

**Real-World Connection**: Arrays model sequential data everywhere: user lists, transaction histories, time series data, game boards, image pixels. Learning array algorithms prepares you for handling ordered collections efficiently.

---

## 📚 Table of Contents

1. [Introduction](#introduction)
2. [Theory & Concepts](#theory--concepts)
3. [Python Implementation](#python-implementation)
4. [Common Patterns](#common-patterns)
5. [Time & Space Complexity](#time--space-complexity)
6. [Common Problems](#common-problems)
7. [Interview Tips](#interview-tips)
8. [Practice Problems](#practice-problems)

---

## Introduction

**What Are Arrays?**

An array is a collection of elements stored in **contiguous memory locations**. Think of it like a row of mailboxes—each mailbox has a number (index) and holds one item. Because the mailboxes are in a row, if you know where the first one is and which number you want, you can calculate exactly where that mailbox is instantly.

**Arrays vs Python Lists**: In languages like C++ or Java, arrays have a fixed size set at creation. Python's `list` is actually a **dynamic array**—it automatically resizes when needed, giving you the flexibility of a list with the performance of an array.

### Key Characteristics

Let's understand why these characteristics matter:

- ✅ **Fast access** by index: O(1)
  - *Why*: Memory address = start_address + (index × element_size)
  - *Example*: `arr[5]` is instant, whether array has 10 or 10 million elements
  - *Interview impact*: Makes arrays perfect when you need quick lookups

- ✅ **Cache-friendly** (contiguous memory)
  - *Why*: CPU loads nearby memory into cache, so iterating is extremely fast
  - *Example*: Looping through an array is 10-100x faster than a linked list
  - *Interview impact*: Arrays beat linked lists for most sequential operations

- ❌ **Slow insertion/deletion** in middle: O(n)
  - *Why*: Must shift all elements after insertion/deletion point
  - *Example*: Inserting at index 0 requires shifting entire array
  - *Interview impact*: If frequent insertions needed, consider linked lists or other structures

- ❌ **Fixed size** (traditional arrays, not Python lists)
  - *Why*: Memory is allocated at creation time
  - *Example*: C++ `int arr[100]` cannot grow beyond 100 elements
  - *Interview impact*: Python lists solve this, but worth knowing for C++/Java interviews

### When to Use

**Choose arrays/lists when**:
- **Need fast random access**: Looking up by index is your main operation
  - *Example*: "Find the kth largest element" → arrays are perfect
  
- **Know approximate size beforehand**: Or can accept occasional resizing
  - *Example*: Reading all values from input → use list
  
- **Iterating through elements sequentially**: Cache-friendly memory access
  - *Example*: "Find sum of all elements" → arrays are optimal
  
- **Implementing other data structures**: Arrays are building blocks
  - *Example*: Stacks, queues, heaps all typically use arrays internally

**Avoid arrays when**:
- Frequent insertions/deletions in middle (use linked list)
- Need guaranteed O(1) insert/delete (use hash set)
- Unknown maximum size and can't afford resizing (use linked list)

---

## Theory & Concepts

### Array vs List (Python)

**Understanding the Difference**

While we often use "array" and "list" interchangeably in Python, they're technically different:

```python
# Arrays (fixed size, same type - from array module)
# Rarely used in Python DSA interviews
import array
arr = array.array('i', [1, 2, 3, 4, 5])  # 'i' = signed int
# Pros: Memory efficient, all elements same type
# Cons: Limited functionality, not idiomatic Python

# Lists (dynamic size, any type - built-in)
# This is what you'll use 99% of the time
lst = [1, 2, 3, 4, 5]
mixed = [1, "two", 3.0, [4, 5]]  # Different types OK

# 💡 Interview Note: When interviewers say "array", they mean Python list
# Real Python arrays are rarely asked about
```

**Why Python Lists Are Special**:
- Dynamic resizing (no need to declare size)
- Can hold mixed types (though usually don't in DSA)
- Rich built-in methods (sort, reverse, etc.)
- Implemented in C for speed

### How Dynamic Arrays Work

**The Resizing Magic**

Understanding this helps you know when operations are expensive:

```
Initial capacity: [1, 2, _, _]  capacity=4, size=2
(Python allocates more space than needed)

Append 3: [1, 2, 3, _]  size=3 ✓
(Fast! Just add to existing space)

Append 4: [1, 2, 3, 4]  size=4 ✓ (full!)
(Still fast, using last slot)

Append 5: Array is full! Time to resize:
          Step 1: Allocate new array (usually 2x size): [_, _, _, _, _, _, _, _]
          Step 2: Copy all elements: [1, 2, 3, 4, 5, _, _, _]
          Step 3: Delete old array
          Size=5, capacity=8 ✓

This resize is O(n), but happens rarely!
Amortized cost of append: O(1) ✓
```

**Why Amortized O(1)**:
- Most appends: O(1) - just add to empty slot
- Occasional resize: O(n) - copy all elements
- Over n appends: Total cost is ~2n operations
- Average per append: 2n/n = 2 = O(1) ✓

**💡 Interview Insight**: This is why building an array by appending n times is O(n) total, not O(n²)!

---

## Python Implementation

### Creating Lists

**Different Ways to Initialize Lists**

Understanding list creation is crucial because initialization bugs are common interview mistakes:

```python
# Method 1: Empty list
empty = []              # Most common
empty = list()          # Same thing, more explicit

# Method 2: With elements - direct initialization
nums = [1, 2, 3, 4, 5]  # Simple and clear

# Method 3: Repeated elements
zeros = [0] * 10        # [0, 0, 0, ..., 0] (10 times)
# ⚠️ Careful: Works for immutable types (int, str, tuple)
# For mutable types, all elements reference the SAME object!

# Method 4: Using range()
numbers = list(range(10))        # [0, 1, 2, ..., 9]
evens = list(range(0, 20, 2))    # [0, 2, 4, ..., 18]
countdown = list(range(10, 0, -1))  # [10, 9, 8, ..., 1]

# 💡 Interview Tip: range() is memory efficient - it generates values on demand

# Method 5: List comprehension (Pythonic!)
squares = [x**2 for x in range(10)]  # [0, 1, 4, 9, ..., 81]
filtered = [x for x in range(20) if x % 2 == 0]  # [0, 2, 4, ..., 18]

# Why use comprehensions?
# - More readable than equivalent for loop
# - Usually faster (optimized at C level)
# - Returns a list directly

# Method 6: 2D List (Matrix) - CRITICAL FOR INTERVIEWS
# ✅ CORRECT way - each row is independent
matrix = [[0] * 3 for _ in range(3)]
# Result: [[0, 0, 0],
#          [0, 0, 0],
#          [0, 0, 0]]
# Modifying matrix[0][0] only affects first row

# ❌ WRONG way - all rows are the SAME object!
wrong = [[0] * 3] * 3
# Modifying wrong[0][0] changes ALL rows!
# This is because * repeats the REFERENCE, not the VALUE

# 💡 Common Interview Bug: Always use comprehension for 2D arrays!
# Pattern: [[initial_value] * cols for _ in range(rows)]
```

### Accessing Elements

**Indexing and Slicing Mastery**

Python's indexing is more powerful than most languages. Master these patterns:

```python
nums = [10, 20, 30, 40, 50]

# Positive indexing (starts at 0)
first = nums[0]          # 10 - first element
second = nums[1]         # 20 - second element
# Why 0-based? Memory address = start + (index × element_size)

# Negative indexing (starts at -1 from end)
last = nums[-1]          # 50 - last element
second_last = nums[-2]   # 40 - second from end
# 💡 Interview Tip: nums[-1] is cleaner than nums[len(nums)-1]

# Slicing [start:end:step]
# Format: list[start:end:step]
# - start: inclusive (default: 0)
# - end: exclusive (default: len)
# - step: increment (default: 1)

subset = nums[1:4]       # [20, 30, 40] - indices 1, 2, 3 (not 4!)
first_three = nums[:3]   # [10, 20, 30] - omit start = from beginning
last_two = nums[-2:]     # [40, 50] - omit end = to end
middle = nums[1:-1]      # [20, 30, 40] - exclude first and last

# Step parameter - skip elements
every_other = nums[::2]  # [10, 30, 50] - every 2nd element
reverse = nums[::-1]     # [50, 40, 30, 20, 10] - step=-1 reverses!

# 💡 Common Interview Patterns:
# - Reverse array: arr[::-1]
# - Copy array: arr[:]
# - Remove first/last: arr[1:] or arr[:-1]
# - First half: arr[:len(arr)//2]
# - Second half: arr[len(arr)//2:]

# Copying - UNDERSTAND THE DIFFERENCE!
shallow_copy = nums[:]  # New list, but elements reference same objects
# For simple types (int, str), behaves like deep copy
# For nested structures, changes to nested objects affect both!

# Deep copy - recursive copying
import copy
deep_copy = copy.deepcopy(nums)  # Completely independent copy

# 💡 Interview Note: For 1D arrays of primitives, [:] is sufficient
# For 2D arrays or nested structures, use deepcopy or comprehension
```

### Modifying Lists

**Operations to Change List Content**

Understanding time complexity of each operation helps you write efficient code:

```python
nums = [1, 2, 3, 4, 5]

# Update element - O(1)
nums[0] = 10  # [10, 2, 3, 4, 5]
# Direct access by index - instant!

# Append (add to end) - O(1) amortized
nums.append(6)  # [10, 2, 3, 4, 5, 6]
# Why amortized O(1)? Usually just adds to empty slot
# Occasionally needs to resize (O(n)), but rare enough that average is O(1)
# 💡 Interview Tip: Building array by appending n times = O(n) total

# Insert (at specific index) - O(n)
nums.insert(0, 0)  # [0, 10, 2, 3, 4, 5, 6]
# Why O(n)? Must shift all elements after insertion point
# Inserting at index i means shifting (n-i) elements
# Worst case: insert(0, x) shifts entire array!
# 💡 Avoid inserting at beginning if possible - it's expensive!

# Extend (add multiple elements) - O(k) where k = number added
nums.extend([7, 8, 9])  # [..., 6, 7, 8, 9]
# More efficient than multiple appends if you have all elements
# Alternative: nums += [7, 8, 9]  (same thing)

# Remove (by value, first occurrence) - O(n)
nums.remove(10)  # Removes first 10 found
# Why O(n)? Must:
#   1. Search for value: O(n)
#   2. Shift elements after deletion: O(n)
# Raises ValueError if element not found!
# 💡 Interview Tip: Use try-except if element might not exist

# Pop (remove by index, returns value) - O(1) or O(n)
last = nums.pop()       # Remove & return last element - O(1)
first = nums.pop(0)     # Remove & return first element - O(n)
# Why different complexities?
# - pop(): Just decrement size, O(1)
# - pop(0): Must shift all remaining elements, O(n)
# 💡 Use pop() for stack (LIFO), use deque for queue (FIFO)

# Delete (by index or slice) - O(n)
del nums[2]  # Delete element at index 2
# Must shift elements after deletion point
del nums[1:3]  # Delete slice
# 💡 del is a statement, not a method - can delete variables too

# Clear all elements - O(1)
nums.clear()  # Empty the list
# Just resets internal pointers, doesn't actually delete memory immediately
# Alternative: nums = [] (but creates new list object)
```

**💡 Key Takeaway for Interviews**:
- **End operations (append, pop)**: O(1) - fast!
- **Beginning operations (insert(0), pop(0))**: O(n) - slow!
- **Middle operations**: O(n) - have to shift elements

**When operation order matters**:
- Need O(1) at both ends? Use `collections.deque`
- Only need O(1) at one end? Regular list is fine

### List Operations

**Built-in Methods for Common Tasks**

Python lists have powerful built-in methods - use them instead of manual loops:

```python
nums = [3, 1, 4, 1, 5, 9, 2, 6, 5]

# Length - O(1)
length = len(nums)  # 9
# Length is stored internally, not calculated!

# Sum, Min, Max - O(n)
total = sum(nums)       # 36 - iterates through all elements
minimum = min(nums)     # 1  - finds smallest
maximum = max(nums)     # 9  - finds largest

# Count occurrences - O(n)
count_ones = nums.count(1)  # 2 - how many times 1 appears
# Iterates through entire list

# Find index - O(n)
index_of_4 = nums.index(4)  # 2 - index of first occurrence
# Raises ValueError if not found!
# Can specify range: nums.index(5, 5, 9) - search from index 5 to 9

# 💡 Interview Pattern: Check existence before finding index
if 4 in nums:  # O(n)
    idx = nums.index(4)  # O(n)
# Total: O(n) + O(n) = O(n), but traverses twice
# Better: Use try-except or dict/set for frequent lookups

# Sort - O(n log n)
nums.sort()  # Sorts in-place, returns None
# [1, 1, 2, 3, 4, 5, 5, 6, 9]
# Uses Timsort - hybrid merge sort + insertion sort
# Stable: equal elements keep relative order

nums.sort(reverse=True)  # Sort descending
# [9, 6, 5, 5, 4, 3, 2, 1, 1]

# Custom sorting with key function
nums.sort(key=lambda x: abs(x - 5))  # Sort by distance from 5
# key function called once per element

# Sorted - O(n log n)
original = [3, 1, 4, 1, 5]
sorted_copy = sorted(original)  # Returns new sorted list
# original is unchanged: [3, 1, 4, 1, 5]
# sorted_copy is: [1, 1, 3, 4, 5]

# Reverse - O(n)
nums.reverse()  # Reverse in-place
# Alternative: nums = nums[::-1]  (creates new list)

# Check membership - O(n)
exists = 5 in nums  # True if 5 is in list
not_exists = 10 not in nums  # True if 10 is NOT in list
# 💡 If checking membership frequently, use set: O(1) average
```

**💡 Interview Optimization Tips**:
1. **Multiple lookups needed**: Convert to set first
   ```python
   # Slow: O(n) for each lookup
   for val in queries:
       if val in nums:  # O(n) each time
           process(val)
   
   # Fast: O(n) to create set, O(1) for each lookup
   nums_set = set(nums)  # O(n)
   for val in queries:
       if val in nums_set:  # O(1) each time
           process(val)
   ```

2. **Need sorted + original**: Use `sorted()` not `sort()`
   ```python
   original = [3, 1, 4]
   sorted_nums = sorted(original)  # Keep both
   ```

3. **Custom comparisons**: Use `key=` parameter
   ```python
   # Sort strings by length, then alphabetically
   words.sort(key=lambda s: (len(s), s))
   ```

---

## Common Patterns

**Recognizing Array Patterns in Interview Problems**

These patterns appear in 80%+ of array interview questions. Learning to recognize them is the key to solving unfamiliar problems quickly.

**How to Use This Section**: For each pattern, understand:
1. **When to use it** - What problem characteristics signal this pattern?
2. **The core template** - Memorize the skeleton code
3. **Common variations** - How the pattern adapts to different problems

---

---

### Pattern 1: Two Pointers

**When to Use**:
- Array is sorted (or can be sorted)
- Need to find pairs/triplets that meet a condition
- Problem asks about elements from both ends
- "Find two elements that..." questions

**How It Works**: Start with one pointer at the beginning and one at the end. Move pointers based on the current sum/product/comparison relative to your target. This reduces O(n²) brute force to O(n).

**Template**:
```python
def two_pointer_template(arr):
    left, right = 0, len(arr) - 1
    
    while left < right:
        # Calculate current value/sum/product
        current = some_function(arr[left], arr[right])
        
        if current == target:
            # Found answer!
            return [left, right]
        elif current < target:
            left += 1  # Need larger value
        else:
            right -= 1  # Need smaller value
    
    return []  # No solution found
```

**Example: Two Sum in Sorted Array**

```python
def two_sum_sorted(nums: list[int], target: int) -> list[int]:
    """
    Find two numbers that add up to target in sorted array.
    
    Why this works:
    - If sum is too small, left number needs to be bigger → move left pointer right
    - If sum is too big, right number needs to be smaller → move right pointer left
    - If sum is perfect, we found it!
    
    Time: O(n) - Each pointer moves at most n times
    Space: O(1) - Only using two pointers
    """
    left, right = 0, len(nums) - 1
    
    while left < right:
        current_sum = nums[left] + nums[right]
        
        if current_sum == target:
            return [left, right]  # Found it!
        elif current_sum < target:
            left += 1  # Need larger sum, increase left value
        else:
            right -= 1  # Need smaller sum, decrease right value
    
    return []  # No valid pair exists

# Example walkthrough
# nums = [2, 7, 11, 15], target = 9
# Step 1: left=0(2), right=3(15), sum=17 > 9 → move right
# Step 2: left=0(2), right=2(11), sum=13 > 9 → move right
# Step 3: left=0(2), right=1(7), sum=9 = 9 → Found! [0, 1]

# Test
nums = [2, 7, 11, 15]
print(two_sum_sorted(nums, 9))  # [0, 1]
```

**💡 Interview Variations**:
- **Two Sum** (sorted): Use two pointers
- **Two Sum** (unsorted): Use hash map instead!
- **Pair with given difference**: Same pattern, different condition
- **Container with most water**: Two pointers, move the smaller height

---

---

### Pattern 2: Sliding Window

**When to Use**:
- Problem asks about "subarray" or "substring"
- Need to track contiguous elements
- "Find maximum/minimum of all subarrays of size k"
- "Longest/shortest substring with condition X"

**How It Works**: Maintain a "window" that slides across the array. Instead of recalculating everything for each window position, we add the new element and remove the old element—much more efficient!

**Template**:
```python
def sliding_window_fixed(arr, k):
    """Fixed-size window"""
    # Initialize first window
    window_sum = sum(arr[:k])
    result = window_sum
    
    # Slide window
    for i in range(k, len(arr)):
        window_sum += arr[i]      # Add new element
        window_sum -= arr[i - k]  # Remove old element
        result = max(result, window_sum)
    
    return result

def sliding_window_variable(arr):
    """Variable-size window"""
    left = 0
    max_length = 0
    
    for right in range(len(arr)):
        # Add arr[right] to window
        
        # Shrink window while condition violated
        while condition_violated:
            # Remove arr[left] from window
            left += 1
        
        # Update result
        max_length = max(max_length, right - left + 1)
    
    return max_length
```

**Example: Maximum Sum Subarray of Size K**

```python
def max_sum_subarray(nums: list[int], k: int) -> int:
    """
    Find maximum sum of any contiguous subarray of size k.
    
    Brute Force Approach (DON'T DO THIS):
    - For each starting position, sum next k elements
    - Time: O(n * k) - Too slow!
    
    Sliding Window Approach (OPTIMAL):
    - Calculate sum of first k elements: O(k)
    - Slide window: remove leftmost, add rightmost: O(1) per slide
    - Total: O(n) - Much better!
    
    Why it works:
    Window: [1, 4, 2, 10]  sum=17
    Slide right →
    New window: [4, 2, 10, 23]  sum = 17 - 1 + 23 = 39
    We reuse the calculation instead of summing from scratch!
    
    Time: O(n), Space: O(1)
    """
    if len(nums) < k:
        return 0
    
    # Step 1: Calculate sum of first window
    window_sum = sum(nums[:k])
    max_sum = window_sum
    
    # Step 2: Slide window across rest of array
    for i in range(k, len(nums)):
        # Slide window: remove leftmost, add rightmost
        window_sum = window_sum - nums[i - k] + nums[i]
        max_sum = max(max_sum, window_sum)
    
    return max_sum

# Example walkthrough
# nums = [1, 4, 2, 10, 23, 3, 1, 0, 20], k = 4
# Window 1: [1,4,2,10] → sum=17
# Window 2: [4,2,10,23] → sum=17-1+23=39 (new max!)
# Window 3: [2,10,23,3] → sum=39-4+3=38
# ... and so on
# Maximum: 39

# Test
nums = [1, 4, 2, 10, 23, 3, 1, 0, 20]
print(max_sum_subarray(nums, 4))  # 39
```

**💡 Interview Variations**:
- **Fixed window**: Maximum/minimum of subarrays of size k
- **Variable window**: Longest substring without repeating characters
- **With hash map**: Longest substring with at most k distinct characters
- **With counter**: Permutation in string problems

---

---

### Pattern 3: Prefix Sum

**When to Use**:
- Multiple queries for sum of subarray ranges
- Need cumulative information (sum, product, XOR)
- "Sum of elements between index i and j" asked repeatedly
- Can trade O(n) preprocessing for O(1) queries

**How It Works**: Precompute cumulative sums so that any range sum becomes a simple subtraction. Instead of summing elements from index i to j every time (O(n)), we calculate: `prefix[j+1] - prefix[i]` in O(1)!

**Why It's Brilliant**:
```
Array:       [3, 1, 4, 1, 5, 9, 2, 6]
Prefix sum:  [0, 3, 4, 8, 9,14,23,25,31]
             ↑  ↑
             0  sum(arr[0:0]) = 0
                sum(arr[0:1]) = 3
                sum(arr[0:2]) = 3+1 = 4
                ...
                sum(arr[0:8]) = 31

Sum from index 2 to 5:
= prefix[6] - prefix[2]
= 23 - 4  
= 19
= 4 + 1 + 5 + 9 ✓
```

**Template**:
```python
def build_prefix_sum(arr):
    """Build prefix sum array"""
    prefix = [0]  # Start with 0 for easier calculation
    for num in arr:
        prefix.append(prefix[-1] + num)
    return prefix

def range_sum(prefix, left, right):
    """Get sum from index left to right (inclusive)"""
    return prefix[right + 1] - prefix[left]
```

**Example: Range Sum Query**

```python
def range_sum_query(nums: list[int]) -> callable:
    """
    Precompute prefix sums for O(1) range queries.
    
    Without prefix sum:
    - Each query: O(n) - sum all elements in range
    - Q queries: O(Q * n) - very slow for many queries!
    
    With prefix sum:
    - Preprocessing: O(n) - build prefix sum array once
    - Each query: O(1) - just one subtraction!
    - Q queries: O(n + Q) - much better!
    
    Preprocessing: O(n)
    Query: O(1)
    Space: O(n)
    """
    # Build prefix sum array
    # prefix[i] = sum of elements from index 0 to i-1
    prefix = [0]
    for num in nums:
        prefix.append(prefix[-1] + num)
    
    def query(left: int, right: int) -> int:
        """
        Sum of elements from index left to right (inclusive).
        
        Formula: prefix[right + 1] - prefix[left]
        
        Why? 
        prefix[right + 1] = sum(nums[0:right+1])
        prefix[left] = sum(nums[0:left])
        Difference = sum(nums[left:right+1]) ✓
        """
        return prefix[right + 1] - prefix[left]
    
    return query

# Example with detailed walkthrough
# nums = [1, 2, 3, 4, 5]
# Build prefix: [0, 1, 3, 6, 10, 15]
#                ↑  ↑  ↑  ↑   ↑   ↑
#                0  1  3  6  10  15

# Query: Sum from index 1 to 3 (inclusive) → 2+3+4 = 9
# prefix[4] - prefix[1] = 10 - 1 = 9 ✓

# Test
nums = [1, 2, 3, 4, 5]
query = range_sum_query(nums)
print(query(1, 3))  # 9 (2+3+4)
print(query(0, 4))  # 15 (1+2+3+4+5)
print(query(2, 2))  # 3 (just element at index 2)
```

**💡 Interview Applications**:
- **Subarray Sum Equals K**: Use prefix sum + hash map
- **Range Sum Query**: Direct application
- **Equilibrium Index**: Left sum = Right sum
- **Subarrays with Divisible Sum**: Prefix sum modulo technique

---

---

### Pattern 4: Kadane's Algorithm (Maximum Subarray)

**When to Use**:
- "Find maximum sum of contiguous subarray"
- Any problem about "consecutive elements" with sum/product
- Classic DP problem with elegant O(n) solution

**The Problem**: Given an array (possibly with negative numbers), find the contiguous subarray with the largest sum.

**Naive Approach** (Don't do this in interviews!):
```python
# Check all possible subarrays - O(n²) or O(n³)
for i in range(n):
    for j in range(i, n):
        current_sum = sum(arr[i:j+1])  # O(n) here makes it O(n³)!
        max_sum = max(max_sum, current_sum)
```

**Kadane's Brilliant Insight**:
At each position, we have two choices:
1. **Extend** the previous subarray by including current element
2. **Start fresh** from current element

Choose whichever gives a larger sum!

```
Decision at each step:
current_sum = max(nums[i], current_sum + nums[i])
              ↑               ↑
         start fresh    extend previous

If previous sum is negative, it drags us down → start fresh!
If previous sum is positive, it helps us → extend!
```

**Visual Example**:
```
Array: [-2,  1, -3,  4, -1,  2,  1, -5,  4]
        ↓   ↓   ↓   ↓   ↓   ↓   ↓   ↓   ↓
curr:  -2   1  -2   4   3   5   6   1   5
        ↑   ↑   ↑   ↑       ↑       
      start fresh   start  (keep extending from 4)
      from 1        from 4  max=6 at position 6!

Best subarray: [4, -1, 2, 1] = 6
```

**Template**:
```python
def max_subarray_sum(nums):
    max_so_far = float('-inf')
    current_sum = 0
    
    for num in nums:
        # Decide: extend previous or start fresh?
        current_sum = max(num, current_sum + num)
        # Track maximum seen so far
        max_so_far = max(max_so_far, current_sum)
    
    return max_so_far
```

**Complete Implementation with Explanation**:

```python
def max_subarray_sum(nums: list[int]) -> int:
    """
    Find maximum sum of any contiguous subarray (Kadane's Algorithm).
    
    Key Insight:
    At each position i, the maximum subarray ending at i is either:
    1. Just nums[i] alone (start fresh)
    2. nums[i] + (maximum subarray ending at i-1)
    
    We choose whichever is larger!
    
    Why it works:
    - If previous sum is negative, it hurts us → start fresh
    - If previous sum is positive, it helps us → keep it
    
    This is dynamic programming: each decision uses the optimal solution
    to the subproblem ending at the previous position.
    
    Time: O(n) - Single pass through array
    Space: O(1) - Only tracking two variables
    """
    if not nums:
        return 0
    
    # Track two things:
    # 1. Max sum of subarray ending at current position
    # 2. Max sum seen so far (our answer)
    
    max_ending_here = nums[0]  # Best subarray ending at current position
    max_so_far = nums[0]        # Best subarray found so far
    
    for i in range(1, len(nums)):
        # Decision: extend previous subarray or start fresh?
        # If max_ending_here is negative, better to start fresh
        # If max_ending_here is positive, extend it
        max_ending_here = max(nums[i], max_ending_here + nums[i])
        
        # Update global maximum if current is better
        max_so_far = max(max_so_far, max_ending_here)
    
    return max_so_far

# Step-by-step walkthrough
# nums = [-2, 1, -3, 4, -1, 2, 1, -5, 4]
# 
# i=0: max_ending_here = -2, max_so_far = -2
# i=1: max(1, -2+1=-1) = 1, max_so_far = 1
# i=2: max(-3, 1+(-3)=-2) = -2, max_so_far = 1  
# i=3: max(4, -2+4=2) = 4, max_so_far = 4 (start fresh from 4!)
# i=4: max(-1, 4+(-1)=3) = 3, max_so_far = 4 (extend: [4,-1])
# i=5: max(2, 3+2=5) = 5, max_so_far = 5 (extend: [4,-1,2])
# i=6: max(1, 5+1=6) = 6, max_so_far = 6 (extend: [4,-1,2,1])
# i=7: max(-5, 6+(-5)=1) = 1, max_so_far = 6 (extend but worse)
# i=8: max(4, 1+4=5) = 5, max_so_far = 6 (extend but still worse)
#
# Answer: 6 (subarray [4, -1, 2, 1])

# Test
print(max_subarray_sum([-2, 1, -3, 4, -1, 2, 1, -5, 4]))  # 6
print(max_subarray_sum([1]))  # 1
print(max_subarray_sum([5, 4, -1, 7, 8]))  # 23 (entire array)
```

**💡 Interview Variations**:
- **Return the subarray itself** (not just sum): Track start/end indices
- **Maximum product subarray**: Similar idea but track both max and min (negatives flip signs!)
- **Circular array**: Run Kadane's twice (normal + inverted)
- **At most K elements**: Add constraint tracking

**Common Follow-ups**:
1. **"What if array is circular?"**
   - Answer: Max of (Kadane's normal, total_sum - Kadane's on inverted)

2. **"Return the actual subarray, not just sum?"**
   - Answer: Track start and end indices when updating max_so_far

3. **"What if all numbers are negative?"**
   - Answer: Algorithm still works - returns the least negative number

---

### Pattern 5: In-Place Array Manipulation

Modify array without using extra space.

```python
def remove_duplicates(nums: list[int]) -> int:
    """
    Remove duplicates from sorted array in-place.
    Returns length of unique elements.
    
    Time: O(n), Space: O(1)
    """
    if not nums:
        return 0
    
    write_idx = 1  # Where to write next unique element
    
    for read_idx in range(1, len(nums)):
        if nums[read_idx] != nums[read_idx - 1]:
            nums[write_idx] = nums[read_idx]
            write_idx += 1
    
    return write_idx

# Example
nums = [1, 1, 2, 2, 2, 3, 4, 4]
length = remove_duplicates(nums)
print(nums[:length])  # [1, 2, 3, 4]
```

### Pattern 6: Dutch National Flag (3-Way Partition)

Partition array into three sections.

```python
def sort_colors(nums: list[int]) -> None:
    """
    Sort array containing 0s, 1s, and 2s in-place.
    
    Time: O(n), Space: O(1)
    """
    low, mid, high = 0, 0, len(nums) - 1
    
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

# Example
nums = [2, 0, 2, 1, 1, 0]
sort_colors(nums)
print(nums)  # [0, 0, 1, 1, 2, 2]
```

---

## Time & Space Complexity

**Understanding Array Operation Costs**

Knowing these complexities helps you choose the right data structure and optimize your solutions. These are the costs for Python lists specifically:

### Common Operations

| Operation | Time | Space | Notes |
|-----------|------|-------|-------|
| **Access by index** | O(1) | O(1) | Direct memory calculation: `base_address + index × size` |
| **Append** | O(1)* | O(1) | *Amortized - occasionally O(n) when resizing |
| **Pop (from end)** | O(1) | O(1) | Just decrement size pointer |
| **Insert at beginning** | O(n) | O(1) | Must shift all n elements right |
| **Insert at position i** | O(n) | O(1) | Must shift (n-i) elements right |
| **Delete from position i** | O(n) | O(1) | Must shift elements left to fill gap |
| **Search (unsorted)** | O(n) | O(1) | Must check each element until found |
| **Binary search (sorted)** | O(log n) | O(1) | Halve search space each step |
| **Sort** | O(n log n) | O(n) | Timsort (Python's default) - stable, adaptive |
| **Reverse** | O(n) | O(1) | In-place swapping from both ends |
| **Slice [i:j]** | O(k) | O(k) | k = slice size, creates new list |
| **Concatenate (+)** | O(n+m) | O(n+m) | Creates new list with all elements |
| **Multiply (*k)** | O(nk) | O(nk) | Creates new list with k repetitions |

**💡 Key Takeaways for Interviews**:
1. **End operations are fast**: append(), pop() → O(1)
2. **Beginning operations are slow**: insert(0, x), pop(0) → O(n)
3. **If you need fast operations at both ends**: Use `collections.deque`
4. **Sorting is expensive**: O(n log n) - only sort if necessary
5. **Slicing creates a copy**: Use indices to avoid copying when possible

**Memory Usage**:
- Python list: ~56 bytes overhead + (8 bytes × capacity)
- Over-allocates for growth: capacity ≈ size × 1.125
- Example: List of 100 integers ≈ 56 + (8 × 112) = 952 bytes

---

## Common Problems

### 1. Two Sum

**Problem**: Find two numbers that add up to target.

```python
def two_sum(nums: list[int], target: int) -> list[int]:
    """
    Time: O(n), Space: O(n)
    """
    seen = {}
    
    for i, num in enumerate(nums):
        complement = target - num
        if complement in seen:
            return [seen[complement], i]
        seen[num] = i
    
    return []

# Test
print(two_sum([2, 7, 11, 15], 9))  # [0, 1]
```

### 2. Best Time to Buy and Sell Stock

**Problem**: Maximize profit from one buy and one sell.

```python
def max_profit(prices: list[int]) -> int:
    """
    Time: O(n), Space: O(1)
    """
    min_price = float('inf')
    max_profit = 0
    
    for price in prices:
        min_price = min(min_price, price)
        max_profit = max(max_profit, price - min_price)
    
    return max_profit

# Test
print(max_profit([7, 1, 5, 3, 6, 4]))  # 5 (buy at 1, sell at 6)
```

### 3. Contains Duplicate

**Problem**: Check if array contains any duplicates.

```python
def contains_duplicate(nums: list[int]) -> bool:
    """
    Time: O(n), Space: O(n)
    """
    return len(nums) != len(set(nums))

# Test
print(contains_duplicate([1, 2, 3, 1]))  # True
print(contains_duplicate([1, 2, 3, 4]))  # False
```

### 4. Product of Array Except Self

**Problem**: Return array where output[i] is product of all elements except nums[i].

```python
def product_except_self(nums: list[int]) -> list[int]:
    """
    Without division, O(n) time, O(1) extra space.
    
    Time: O(n), Space: O(1) (output array doesn't count)
    """
    n = len(nums)
    result = [1] * n
    
    # Left pass: multiply all elements to the left
    left_product = 1
    for i in range(n):
        result[i] = left_product
        left_product *= nums[i]
    
    # Right pass: multiply all elements to the right
    right_product = 1
    for i in range(n - 1, -1, -1):
        result[i] *= right_product
        right_product *= nums[i]
    
    return result

# Test
print(product_except_self([1, 2, 3, 4]))  # [24, 12, 8, 6]
```

### 5. Maximum Subarray (Kadane's Algorithm)

**Problem**: Find contiguous subarray with largest sum.

```python
def max_subarray(nums: list[int]) -> int:
    """
    Time: O(n), Space: O(1)
    """
    max_sum = nums[0]
    current_sum = nums[0]
    
    for i in range(1, len(nums)):
        current_sum = max(nums[i], current_sum + nums[i])
        max_sum = max(max_sum, current_sum)
    
    return max_sum

# Test
print(max_subarray([-2, 1, -3, 4, -1, 2, 1, -5, 4]))  # 6
```

### 6. Rotate Array

**Problem**: Rotate array to the right by k steps.

```python
def rotate(nums: list[int], k: int) -> None:
    """
    Rotate in-place using reversal algorithm.
    
    Time: O(n), Space: O(1)
    """
    n = len(nums)
    k = k % n  # Handle k > n
    
    # Helper function
    def reverse(start: int, end: int) -> None:
        while start < end:
            nums[start], nums[end] = nums[end], nums[start]
            start += 1
            end -= 1
    
    # Reverse entire array
    reverse(0, n - 1)
    # Reverse first k elements
    reverse(0, k - 1)
    # Reverse remaining elements
    reverse(k, n - 1)

# Test
nums = [1, 2, 3, 4, 5, 6, 7]
rotate(nums, 3)
print(nums)  # [5, 6, 7, 1, 2, 3, 4]
```

### 7. Find Minimum in Rotated Sorted Array

**Problem**: Find minimum in rotated sorted array.

```python
def find_min(nums: list[int]) -> int:
    """
    Time: O(log n), Space: O(1)
    """
    left, right = 0, len(nums) - 1
    
    while left < right:
        mid = (left + right) // 2
        
        if nums[mid] > nums[right]:
            # Minimum is in right half
            left = mid + 1
        else:
            # Minimum is in left half (including mid)
            right = mid
    
    return nums[left]

# Test
print(find_min([3, 4, 5, 1, 2]))  # 1
print(find_min([4, 5, 6, 7, 0, 1, 2]))  # 0
```

### 8. 3Sum

**Problem**: Find all unique triplets that sum to zero.

```python
def three_sum(nums: list[int]) -> list[list[int]]:
    """
    Time: O(n²), Space: O(1) excluding output
    """
    nums.sort()
    result = []
    
    for i in range(len(nums) - 2):
        # Skip duplicates for first element
        if i > 0 and nums[i] == nums[i - 1]:
            continue
        
        # Two pointers for remaining two elements
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

---

## Interview Tips

### 1. Clarify Constraints
- Array sorted or unsorted?
- Can modify input array?
- Duplicates allowed?
- Array size limits?

### 2. Common Tricks
```python
# Negative indexing
last = arr[-1]

# Swapping without temp
a, b = b, a

# Check if array is sorted
is_sorted = all(arr[i] <= arr[i+1] for i in range(len(arr)-1))

# Multiple pointers
left, right = 0, len(arr) - 1
```

### 3. Edge Cases
```python
# Always consider:
- Empty array: []
- Single element: [1]
- Two elements: [1, 2]
- All same elements: [5, 5, 5, 5]
- Negative numbers
- Large numbers
```

### 4. Optimization Checklist
- ✅ Can sort help? (O(n log n))
- ✅ Can hash map help? (Trade space for time)
- ✅ Can process in single pass?
- ✅ Can use two pointers?
- ✅ Can preprocess data?

---

## Practice Problems

### Easy
1. [Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)
2. [Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array/)
3. [Move Zeroes](https://leetcode.com/problems/move-zeroes/)
4. [Plus One](https://leetcode.com/problems/plus-one/)

### Medium
1. [Container With Most Water](https://leetcode.com/problems/container-with-most-water/)
2. [3Sum](https://leetcode.com/problems/3sum/)
3. [Sort Colors](https://leetcode.com/problems/sort-colors/)
4. [Find Peak Element](https://leetcode.com/problems/find-peak-element/)

### Hard
1. [First Missing Positive](https://leetcode.com/problems/first-missing-positive/)
2. [Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/)
3. [Median of Two Sorted Arrays](https://leetcode.com/problems/median-of-two-sorted-arrays/)

---

## Summary

Arrays/Lists are fundamental to DSA. Master these concepts:
- ✅ Two pointers technique
- ✅ Sliding window
- ✅ Prefix sum
- ✅ In-place manipulation
- ✅ Binary search (if sorted)

### Quick Reference
```python
# Creation
arr = [1, 2, 3]

# Access
arr[0], arr[-1], arr[1:3]

# Modify
arr.append(4), arr.insert(0, 0), arr.pop()

# Search
x in arr, arr.index(x), arr.count(x)

# Sort
arr.sort(), sorted(arr)
```

---

**Next**: [Strings →](../02-strings/README.md)

**Happy Coding! 🚀**
