# 🔑 Hash Tables - Python DSA

> O(1) average-case lookup, insertion, and deletion

---

## 📚 Table of Contents

1. [Introduction](#introduction)
2. [How Hash Tables Work](#how-hash-tables-work)
3. [Python Implementation](#python-implementation)
4. [Common Patterns](#common-patterns)
5. [Classic Problems](#classic-problems)
6. [Interview Tips](#interview-tips)
7. [Practice Problems](#practice-problems)

---

## Introduction

**Hash Table** (Hash Map) is a data structure that maps keys to values using a hash function.

### Key Characteristics
- ✅ **O(1) average lookup** - Constant time access
- ✅ **O(1) average insertion** - Fast add/update
- ✅ **O(1) average deletion** - Fast removal
- ✅ **Key-value pairs** - Associate data
- ❌ **No ordering** - Elements not sorted
- ❌ **Extra space** - Hash table overhead

### Real-World Examples
- 📖 Dictionary (word → definition)
- 📞 Phone book (name → number)
- 🗄️ Database index (key → record)
- 🔐 Password storage (username → hash)
- 🌐 DNS (domain → IP address)

### Hash Table Operations

| Operation | Average | Worst | Description |
|-----------|---------|-------|-------------|
| Search | O(1) | O(n) | Find value |
| Insert | O(1) | O(n) | Add key-value |
| Delete | O(1) | O(n) | Remove key |
| Update | O(1) | O(n) | Modify value |

*Worst case occurs with many collisions*

---

## How Hash Tables Work

### 1. Hash Function

Converts key to array index.

```python
def hash_function(key, size):
    """Simple hash function."""
    return hash(key) % size

# Example:
# hash("apple") % 10 → 3
# hash("banana") % 10 → 7
```

### 2. Collision Resolution

#### Chaining (Separate Chaining)
Each bucket contains a linked list of entries.

```
Index  Bucket
0      → None
1      → [("key1", val1)] → [("key9", val9)]
2      → [("key2", val2)]
3      → None
```

#### Open Addressing (Linear Probing)
Find next available slot.

```
Index  Key      Value
0      "key1"   val1
1      "key2"   val2
2      None     None
3      "key3"   val3
```

### 3. Load Factor

```python
load_factor = number_of_elements / table_size

# Rehash when load_factor > 0.7
```

---

## Python Implementation

### Using dict (Built-in)

```python
# Create hash table
hash_map = {}

# Insert
hash_map["apple"] = 5
hash_map["banana"] = 3

# Access
print(hash_map["apple"])  # 5

# Update
hash_map["apple"] = 10

# Delete
del hash_map["banana"]

# Check existence
if "apple" in hash_map:
    print("Found!")

# Get with default
value = hash_map.get("orange", 0)  # Returns 0 if not found

# Iterate
for key, value in hash_map.items():
    print(f"{key}: {value}")
```

### Using defaultdict

```python
from collections import defaultdict

# Default value for missing keys
word_count = defaultdict(int)
word_count["apple"] += 1  # No KeyError!

# Group by key
groups = defaultdict(list)
groups["fruit"].append("apple")
groups["fruit"].append("banana")
```

### Using Counter

```python
from collections import Counter

# Count elements
nums = [1, 2, 2, 3, 3, 3]
counter = Counter(nums)
print(counter)  # Counter({3: 3, 2: 2, 1: 1})

# Most common
print(counter.most_common(2))  # [(3, 3), (2, 2)]

# Operations
a = Counter([1, 2, 3])
b = Counter([2, 3, 4])
print(a + b)  # Counter({2: 2, 3: 2, 1: 1, 4: 1})
print(a & b)  # Counter({2: 1, 3: 1})  # Intersection
```

### Custom Hash Table (Chaining)

```python
class HashTable:
    """Hash table with separate chaining."""
    
    def __init__(self, size=10):
        self.size = size
        self.table = [[] for _ in range(size)]
        self.count = 0
    
    def _hash(self, key):
        """Hash function."""
        return hash(key) % self.size
    
    def put(self, key, value):
        """Insert or update key-value pair. Time: O(1) average"""
        index = self._hash(key)
        bucket = self.table[index]
        
        # Update if key exists
        for i, (k, v) in enumerate(bucket):
            if k == key:
                bucket[i] = (key, value)
                return
        
        # Insert new key
        bucket.append((key, value))
        self.count += 1
        
        # Rehash if needed
        if self.count / self.size > 0.7:
            self._rehash()
    
    def get(self, key):
        """Get value by key. Time: O(1) average"""
        index = self._hash(key)
        bucket = self.table[index]
        
        for k, v in bucket:
            if k == key:
                return v
        
        raise KeyError(f"Key {key} not found")
    
    def remove(self, key):
        """Remove key. Time: O(1) average"""
        index = self._hash(key)
        bucket = self.table[index]
        
        for i, (k, v) in enumerate(bucket):
            if k == key:
                bucket.pop(i)
                self.count -= 1
                return
        
        raise KeyError(f"Key {key} not found")
    
    def contains(self, key):
        """Check if key exists. Time: O(1) average"""
        index = self._hash(key)
        bucket = self.table[index]
        
        return any(k == key for k, v in bucket)
    
    def _rehash(self):
        """Resize and rehash when load factor too high."""
        old_table = self.table
        self.size *= 2
        self.table = [[] for _ in range(self.size)]
        self.count = 0
        
        for bucket in old_table:
            for key, value in bucket:
                self.put(key, value)
    
    def __len__(self):
        return self.count
    
    def __repr__(self):
        items = []
        for bucket in self.table:
            items.extend(bucket)
        return f"HashTable({dict(items)})"

# Test
ht = HashTable()
ht.put("apple", 5)
ht.put("banana", 3)
print(ht.get("apple"))  # 5
```

---

## Common Patterns

### Pattern 1: Frequency Counter

```python
def char_frequency(s: str) -> dict:
    """
    Count character frequencies.
    
    Time: O(n)
    Space: O(k) where k is unique characters
    """
    freq = {}
    for char in s:
        freq[char] = freq.get(char, 0) + 1
    return freq

# Or using Counter
from collections import Counter
freq = Counter(s)
```

### Pattern 2: Two Sum Pattern

```python
def two_sum(nums: list[int], target: int) -> list[int]:
    """
    Find two numbers that sum to target.
    
    Time: O(n)
    Space: O(n)
    """
    seen = {}  # value → index
    
    for i, num in enumerate(nums):
        complement = target - num
        if complement in seen:
            return [seen[complement], i]
        seen[num] = i
    
    return []
```

### Pattern 3: Group/Categorize

```python
def group_anagrams(words: list[str]) -> list[list[str]]:
    """
    Group anagrams together.
    
    Time: O(n * k log k) where k is max word length
    Space: O(n * k)
    """
    from collections import defaultdict
    
    groups = defaultdict(list)
    
    for word in words:
        key = ''.join(sorted(word))
        groups[key].append(word)
    
    return list(groups.values())
```

### Pattern 4: Sliding Window with Hash Map

```python
def longest_substring_without_repeating(s: str) -> int:
    """
    Find longest substring without repeating characters.
    
    Time: O(n)
    Space: O(min(n, m)) where m is charset size
    """
    char_index = {}
    max_length = 0
    start = 0
    
    for end, char in enumerate(s):
        if char in char_index and char_index[char] >= start:
            start = char_index[char] + 1
        
        char_index[char] = end
        max_length = max(max_length, end - start + 1)
    
    return max_length
```

---

## Classic Problems

### 1. Two Sum

```python
def two_sum(nums: list[int], target: int) -> list[int]:
    """
    Time: O(n)
    Space: O(n)
    """
    seen = {}
    
    for i, num in enumerate(nums):
        complement = target - num
        if complement in seen:
            return [seen[complement], i]
        seen[num] = i
    
    return []

# Example:
# Input: nums = [2,7,11,15], target = 9
# Output: [0,1]
```

### 2. Group Anagrams

```python
def group_anagrams(strs: list[str]) -> list[list[str]]:
    """
    Time: O(n * k log k)
    Space: O(n * k)
    """
    from collections import defaultdict
    
    groups = defaultdict(list)
    
    for s in strs:
        # Use sorted string as key
        key = ''.join(sorted(s))
        groups[key].append(s)
    
    return list(groups.values())

# Alternative: Using character count as key
def group_anagrams_v2(strs: list[str]) -> list[list[str]]:
    """
    Time: O(n * k)
    Space: O(n * k)
    """
    from collections import defaultdict
    
    groups = defaultdict(list)
    
    for s in strs:
        # Character count as key
        count = [0] * 26
        for char in s:
            count[ord(char) - ord('a')] += 1
        groups[tuple(count)].append(s)
    
    return list(groups.values())
```

### 3. Valid Anagram

```python
def is_anagram(s: str, t: str) -> bool:
    """
    Time: O(n)
    Space: O(1) - fixed 26 letters
    """
    if len(s) != len(t):
        return False
    
    from collections import Counter
    return Counter(s) == Counter(t)

# Alternative: Manual counting
def is_anagram_v2(s: str, t: str) -> bool:
    """
    Time: O(n)
    Space: O(1)
    """
    if len(s) != len(t):
        return False
    
    count = {}
    for char in s:
        count[char] = count.get(char, 0) + 1
    
    for char in t:
        if char not in count:
            return False
        count[char] -= 1
        if count[char] < 0:
            return False
    
    return True
```

### 4. First Unique Character

```python
def first_uniq_char(s: str) -> int:
    """
    Time: O(n)
    Space: O(1) - fixed charset
    """
    from collections import Counter
    
    # Count frequencies
    count = Counter(s)
    
    # Find first unique
    for i, char in enumerate(s):
        if count[char] == 1:
            return i
    
    return -1

# Example:
# Input: s = "leetcode"
# Output: 0
```

### 5. Longest Substring Without Repeating Characters

```python
def length_of_longest_substring(s: str) -> int:
    """
    Time: O(n)
    Space: O(min(n, m))
    """
    char_index = {}
    max_length = 0
    start = 0
    
    for end, char in enumerate(s):
        # If char seen and in current window
        if char in char_index and char_index[char] >= start:
            start = char_index[char] + 1
        
        char_index[char] = end
        max_length = max(max_length, end - start + 1)
    
    return max_length

# Example:
# Input: s = "abcabcbb"
# Output: 3  # "abc"
```

### 6. Subarray Sum Equals K

```python
def subarray_sum(nums: list[int], k: int) -> int:
    """
    Count subarrays with sum equal to k.
    
    Time: O(n)
    Space: O(n)
    """
    count = 0
    prefix_sum = 0
    sum_count = {0: 1}  # prefix_sum → frequency
    
    for num in nums:
        prefix_sum += num
        
        # Check if (prefix_sum - k) exists
        if prefix_sum - k in sum_count:
            count += sum_count[prefix_sum - k]
        
        # Update frequency
        sum_count[prefix_sum] = sum_count.get(prefix_sum, 0) + 1
    
    return count

# Example:
# Input: nums = [1,1,1], k = 2
# Output: 2  # [1,1] and [1,1]
```

### 7. Top K Frequent Elements

```python
def top_k_frequent(nums: list[int], k: int) -> list[int]:
    """
    Time: O(n log k)
    Space: O(n)
    """
    from collections import Counter
    import heapq
    
    # Count frequencies
    count = Counter(nums)
    
    # Use heap to find top k
    return heapq.nlargest(k, count.keys(), key=count.get)

# Alternative: Bucket sort O(n)
def top_k_frequent_v2(nums: list[int], k: int) -> list[int]:
    """
    Time: O(n)
    Space: O(n)
    """
    from collections import Counter
    
    count = Counter(nums)
    
    # Bucket sort by frequency
    buckets = [[] for _ in range(len(nums) + 1)]
    for num, freq in count.items():
        buckets[freq].append(num)
    
    # Collect top k
    result = []
    for i in range(len(buckets) - 1, 0, -1):
        result.extend(buckets[i])
        if len(result) >= k:
            return result[:k]
    
    return result
```

### 8. Longest Consecutive Sequence

```python
def longest_consecutive(nums: list[int]) -> int:
    """
    Time: O(n)
    Space: O(n)
    """
    if not nums:
        return 0
    
    num_set = set(nums)
    max_length = 0
    
    for num in num_set:
        # Only start counting from beginning of sequence
        if num - 1 not in num_set:
            current_num = num
            current_length = 1
            
            # Count consecutive numbers
            while current_num + 1 in num_set:
                current_num += 1
                current_length += 1
            
            max_length = max(max_length, current_length)
    
    return max_length

# Example:
# Input: [100, 4, 200, 1, 3, 2]
# Output: 4  # [1, 2, 3, 4]
```

### 9. Valid Sudoku

```python
def is_valid_sudoku(board: list[list[str]]) -> bool:
    """
    Time: O(1) - fixed 9x9
    Space: O(1)
    """
    rows = [set() for _ in range(9)]
    cols = [set() for _ in range(9)]
    boxes = [set() for _ in range(9)]
    
    for r in range(9):
        for c in range(9):
            if board[r][c] == '.':
                continue
            
            num = board[r][c]
            box_index = (r // 3) * 3 + (c // 3)
            
            # Check duplicates
            if num in rows[r] or num in cols[c] or num in boxes[box_index]:
                return False
            
            rows[r].add(num)
            cols[c].add(num)
            boxes[box_index].add(num)
    
    return True
```

### 10. LRU Cache

```python
class LRUCache:
    """
    Least Recently Used Cache.
    """
    
    def __init__(self, capacity: int):
        self.capacity = capacity
        self.cache = {}  # key → node
        
        # Doubly linked list (dummy head and tail)
        self.head = Node(0, 0)
        self.tail = Node(0, 0)
        self.head.next = self.tail
        self.tail.prev = self.head
    
    def get(self, key: int) -> int:
        """Time: O(1)"""
        if key in self.cache:
            node = self.cache[key]
            self._remove(node)
            self._add(node)
            return node.val
        return -1
    
    def put(self, key: int, value: int) -> None:
        """Time: O(1)"""
        if key in self.cache:
            self._remove(self.cache[key])
        
        node = Node(key, value)
        self._add(node)
        self.cache[key] = node
        
        if len(self.cache) > self.capacity:
            # Remove least recently used (from tail)
            lru = self.tail.prev
            self._remove(lru)
            del self.cache[lru.key]
    
    def _remove(self, node):
        """Remove node from linked list."""
        prev_node = node.prev
        next_node = node.next
        prev_node.next = next_node
        next_node.prev = prev_node
    
    def _add(self, node):
        """Add node to head (most recently used)."""
        next_node = self.head.next
        self.head.next = node
        node.prev = self.head
        node.next = next_node
        next_node.prev = node

class Node:
    def __init__(self, key, val):
        self.key = key
        self.val = val
        self.prev = None
        self.next = None

# Test
cache = LRUCache(2)
cache.put(1, 1)
cache.put(2, 2)
print(cache.get(1))  # 1
cache.put(3, 3)      # evicts key 2
print(cache.get(2))  # -1 (not found)
```

### 11. Design HashMap

```python
class MyHashMap:
    """
    Custom HashMap implementation.
    """
    
    def __init__(self):
        self.size = 1000
        self.buckets = [[] for _ in range(self.size)]
    
    def _hash(self, key: int) -> int:
        """Hash function."""
        return key % self.size
    
    def put(self, key: int, value: int) -> None:
        """Time: O(1) average"""
        index = self._hash(key)
        bucket = self.buckets[index]
        
        # Update if exists
        for i, (k, v) in enumerate(bucket):
            if k == key:
                bucket[i] = (key, value)
                return
        
        # Insert new
        bucket.append((key, value))
    
    def get(self, key: int) -> int:
        """Time: O(1) average"""
        index = self._hash(key)
        bucket = self.buckets[index]
        
        for k, v in bucket:
            if k == key:
                return v
        
        return -1
    
    def remove(self, key: int) -> None:
        """Time: O(1) average"""
        index = self._hash(key)
        bucket = self.buckets[index]
        
        for i, (k, v) in enumerate(bucket):
            if k == key:
                bucket.pop(i)
                return
```

### 12. 4Sum II

```python
def four_sum_count(nums1: list[int], nums2: list[int], 
                   nums3: list[int], nums4: list[int]) -> int:
    """
    Count tuples (i,j,k,l) where nums1[i] + nums2[j] + nums3[k] + nums4[l] == 0.
    
    Time: O(n²)
    Space: O(n²)
    """
    from collections import defaultdict
    
    # Store sums of first two arrays
    sum_count = defaultdict(int)
    for a in nums1:
        for b in nums2:
            sum_count[a + b] += 1
    
    # Check sums of last two arrays
    count = 0
    for c in nums3:
        for d in nums4:
            target = -(c + d)
            count += sum_count[target]
    
    return count
```

---

## Interview Tips

### 1. When to Use Hash Table

```python
# Use hash table for:
- O(1) lookup needed
- Count frequencies
- Detect duplicates
- Group/categorize items
- Cache/memoization
- Two sum variants
- Anagram problems
```

### 2. Common Hash Table Patterns

```python
# Pattern 1: Frequency counter
from collections import Counter
freq = Counter(arr)

# Pattern 2: Seen/visited set
seen = set()
if item in seen:
    # duplicate
seen.add(item)

# Pattern 3: Complement lookup
seen = {}
for i, num in enumerate(nums):
    complement = target - num
    if complement in seen:
        return [seen[complement], i]
    seen[num] = i

# Pattern 4: Group by key
from collections import defaultdict
groups = defaultdict(list)
for item in items:
    key = get_key(item)
    groups[key].append(item)
```

### 3. Python Hash Table Types

```python
# dict - General purpose
d = {}
d = {'a': 1, 'b': 2}

# defaultdict - Auto-initialize missing keys
from collections import defaultdict
dd = defaultdict(int)  # Default to 0
dd = defaultdict(list)  # Default to []

# Counter - Count elements
from collections import Counter
c = Counter([1, 2, 2, 3, 3, 3])

# set - For membership testing
s = {1, 2, 3}
if item in s:  # O(1)
```

### 4. Space-Time Trade-off

```python
# Without hash table: O(n²) time, O(1) space
for i in range(len(arr)):
    for j in range(i+1, len(arr)):
        if arr[i] + arr[j] == target:
            return [i, j]

# With hash table: O(n) time, O(n) space
seen = {}
for i, num in enumerate(arr):
    if target - num in seen:
        return [seen[target - num], i]
    seen[num] = i
```

### 5. Hash vs Sort

```python
# Hash approach: O(n) time, O(n) space
def has_duplicates(arr):
    return len(arr) != len(set(arr))

# Sort approach: O(n log n) time, O(1) space
def has_duplicates_sort(arr):
    arr.sort()
    for i in range(len(arr) - 1):
        if arr[i] == arr[i + 1]:
            return True
    return False
```

---

## Practice Problems

### Easy
1. [Two Sum](https://leetcode.com/problems/two-sum/)
2. [Valid Anagram](https://leetcode.com/problems/valid-anagram/)
3. [Contains Duplicate](https://leetcode.com/problems/contains-duplicate/)
4. [First Unique Character](https://leetcode.com/problems/first-unique-character-in-a-string/)
5. [Intersection of Two Arrays](https://leetcode.com/problems/intersection-of-two-arrays/)

### Medium
1. [Group Anagrams](https://leetcode.com/problems/group-anagrams/)
2. [Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/)
3. [Longest Substring Without Repeating](https://leetcode.com/problems/longest-substring-without-repeating-characters/)
4. [Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/)
5. [Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence/)
6. [Valid Sudoku](https://leetcode.com/problems/valid-sudoku/)
7. [4Sum II](https://leetcode.com/problems/4sum-ii/)

### Hard
1. [LRU Cache](https://leetcode.com/problems/lru-cache/)
2. [First Missing Positive](https://leetcode.com/problems/first-missing-positive/)
3. [Substring with Concatenation](https://leetcode.com/problems/substring-with-concatenation-of-all-words/)

---

## Summary

### Key Takeaways
- ✅ Hash table = O(1) average lookup/insert/delete
- ✅ Perfect for frequency counting
- ✅ Trade space for time efficiency
- ✅ Use for duplicate detection
- ✅ Python: dict, set, defaultdict, Counter

### Common Patterns
```python
# 1. Frequency counter
from collections import Counter
freq = Counter(arr)

# 2. Complement/pair lookup
seen = {}
for i, val in enumerate(arr):
    if target - val in seen:
        return [seen[target - val], i]
    seen[val] = i

# 3. Group by key
from collections import defaultdict
groups = defaultdict(list)
for item in items:
    groups[key(item)].append(item)

# 4. Sliding window with hash
char_count = {}
for char in s:
    char_count[char] = char_count.get(char, 0) + 1
```

---

**Next**: [Binary Trees →](../07-binary-trees/README.md)

**Happy Coding! 🚀**
