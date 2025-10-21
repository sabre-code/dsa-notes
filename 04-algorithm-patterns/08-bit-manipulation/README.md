# 🔢 Bit Manipulation - Python DSA

> Master bitwise operations for efficient solutions

---

## 📚 Table of Contents

1. [Introduction](#introduction)
2. [Bitwise Operators](#bitwise-operators)
3. [Common Bit Tricks](#common-bit-tricks)
4. [Classic Problems](#classic-problems)
5. [Interview Tips](#interview-tips)
6. [Practice Problems](#practice-problems)

---

## Introduction

**Bit Manipulation** uses bitwise operations to solve problems efficiently.

### Key Characteristics
- ✅ **Very fast** - Direct hardware operations
- ✅ **Space efficient** - Can represent sets as integers
- ✅ **Elegant solutions** - Often simpler than alternatives
- ✅ **Interview favorite** - Tests low-level understanding

### When to Use
- Need **O(1) operations** on sets
- Working with **binary representations**
- **Space optimization** required
- Keywords: "binary", "XOR", "power of 2", "bits"

---

## Bitwise Operators

### Basic Operators

```python
# AND (&): Both bits must be 1
print(5 & 3)   # 0101 & 0011 = 0001 = 1

# OR (|): At least one bit must be 1
print(5 | 3)   # 0101 | 0011 = 0111 = 7

# XOR (^): Bits must be different
print(5 ^ 3)   # 0101 ^ 0011 = 0110 = 6

# NOT (~): Flip all bits
print(~5)      # ~0101 = 1010 (in two's complement)

# Left Shift (<<): Multiply by 2^n
print(5 << 1)  # 0101 << 1 = 1010 = 10

# Right Shift (>>): Divide by 2^n
print(5 >> 1)  # 0101 >> 1 = 0010 = 2
```

### XOR Properties

```python
# XOR is commutative and associative
a ^ b == b ^ a
(a ^ b) ^ c == a ^ (b ^ c)

# XOR with self is 0
a ^ a == 0

# XOR with 0 is identity
a ^ 0 == a

# XOR twice cancels out
a ^ b ^ b == a

# All numbers XOR together
a ^ b ^ c ^ b ^ a == c  # Duplicates cancel
```

---

## Common Bit Tricks

### 1. Check if Power of 2

```python
def is_power_of_two(n: int) -> bool:
    """
    Check if n is power of 2.
    
    Time: O(1)
    Space: O(1)
    
    Trick: Power of 2 has exactly one bit set
    8 = 1000, 8-1 = 0111, 8 & 7 = 0
    """
    return n > 0 and (n & (n - 1)) == 0

# Test
print(is_power_of_two(8))   # True
print(is_power_of_two(6))   # False
```

### 2. Count Set Bits

```python
def count_bits(n: int) -> int:
    """
    Count number of 1 bits.
    
    Time: O(k) where k is number of set bits
    Space: O(1)
    
    Trick: n & (n-1) removes rightmost set bit
    """
    count = 0
    while n:
        n &= n - 1  # Remove rightmost 1
        count += 1
    return count

# Alternative: Use Python built-in
def count_bits_builtin(n: int) -> int:
    return bin(n).count('1')

# Test
print(count_bits(11))  # 0b1011 has 3 ones
```

### 3. Get/Set/Clear/Toggle Bit

```python
def get_bit(num: int, i: int) -> int:
    """Get bit at position i."""
    return (num >> i) & 1

def set_bit(num: int, i: int) -> int:
    """Set bit at position i to 1."""
    return num | (1 << i)

def clear_bit(num: int, i: int) -> int:
    """Clear bit at position i (set to 0)."""
    return num & ~(1 << i)

def toggle_bit(num: int, i: int) -> int:
    """Toggle bit at position i."""
    return num ^ (1 << i)

# Test
n = 5  # 0101
print(get_bit(n, 2))      # 1
print(set_bit(n, 1))      # 0111 = 7
print(clear_bit(n, 2))    # 0001 = 1
print(toggle_bit(n, 0))   # 0100 = 4
```

### 4. Isolate Rightmost 1 Bit

```python
def isolate_rightmost_one(n: int) -> int:
    """
    Get rightmost set bit.
    
    Trick: n & -n
    """
    return n & -n

# Test
print(bin(12))  # 0b1100
print(bin(isolate_rightmost_one(12)))  # 0b100
```

### 5. Remove Rightmost 1 Bit

```python
def remove_rightmost_one(n: int) -> int:
    """
    Remove rightmost set bit.
    
    Trick: n & (n-1)
    """
    return n & (n - 1)

# Test
print(bin(12))  # 0b1100
print(bin(remove_rightmost_one(12)))  # 0b1000
```

### 6. Swap Two Numbers

```python
def swap_xor(a: int, b: int) -> tuple:
    """
    Swap without temp variable using XOR.
    
    Time: O(1)
    Space: O(1)
    """
    a = a ^ b
    b = a ^ b  # b = (a^b)^b = a
    a = a ^ b  # a = (a^b)^a = b
    return a, b

# Test
print(swap_xor(5, 10))  # (10, 5)
```

---

## Classic Problems

### Problem 1: Single Number

```python
def single_number(nums: list[int]) -> int:
    """
    Find number that appears once (others appear twice).
    
    Time: O(n)
    Space: O(1)
    
    Example: nums = [4,1,2,1,2]
    Output: 4
    
    Trick: XOR cancels duplicates
    """
    result = 0
    for num in nums:
        result ^= num
    return result

# Test
print(single_number([4,1,2,1,2]))  # 4
```

### Problem 2: Single Number II

```python
def single_number_ii(nums: list[int]) -> int:
    """
    Find number that appears once (others appear 3 times).
    
    Time: O(n)
    Space: O(1)
    
    Example: nums = [2,2,3,2]
    Output: 3
    """
    ones, twos = 0, 0
    
    for num in nums:
        twos |= ones & num
        ones ^= num
        threes = ones & twos
        ones &= ~threes
        twos &= ~threes
    
    return ones

# Test
print(single_number_ii([2,2,3,2]))  # 3
```

### Problem 3: Single Number III

```python
def single_number_iii(nums: list[int]) -> list[int]:
    """
    Find two numbers that appear once (others twice).
    
    Time: O(n)
    Space: O(1)
    
    Example: nums = [1,2,1,3,2,5]
    Output: [3,5]
    """
    # XOR all numbers: result = a ^ b
    xor = 0
    for num in nums:
        xor ^= num
    
    # Find rightmost set bit (difference between a and b)
    rightmost_bit = xor & -xor
    
    # Partition numbers based on this bit
    a, b = 0, 0
    for num in nums:
        if num & rightmost_bit:
            a ^= num
        else:
            b ^= num
    
    return [a, b]

# Test
print(single_number_iii([1,2,1,3,2,5]))  # [3,5] or [5,3]
```

### Problem 4: Missing Number

```python
def missing_number(nums: list[int]) -> int:
    """
    Find missing number from 0 to n.
    
    Time: O(n)
    Space: O(1)
    
    Example: nums = [3,0,1]
    Output: 2
    
    Trick: XOR with indices
    """
    result = len(nums)
    for i, num in enumerate(nums):
        result ^= i ^ num
    return result

# Alternative: Sum formula
def missing_number_sum(nums: list[int]) -> int:
    n = len(nums)
    expected_sum = n * (n + 1) // 2
    return expected_sum - sum(nums)

# Test
print(missing_number([3,0,1]))  # 2
```

### Problem 5: Reverse Bits

```python
def reverse_bits(n: int) -> int:
    """
    Reverse bits of 32-bit integer.
    
    Time: O(1)
    Space: O(1)
    
    Example: n = 43261596 (00000010100101000001111010011100)
    Output: 964176192 (00111001011110000010100101000000)
    """
    result = 0
    for i in range(32):
        result = (result << 1) | (n & 1)
        n >>= 1
    return result

# Test
print(reverse_bits(43261596))
```

### Problem 6: Number of 1 Bits (Hamming Weight)

```python
def hamming_weight(n: int) -> int:
    """
    Count set bits.
    
    Time: O(k) where k is number of set bits
    Space: O(1)
    
    Example: n = 11 (00000000000000000000000000001011)
    Output: 3
    """
    count = 0
    while n:
        n &= n - 1
        count += 1
    return count

# Test
print(hamming_weight(11))  # 3
```

### Problem 7: Hamming Distance

```python
def hamming_distance(x: int, y: int) -> int:
    """
    Count different bits between two numbers.
    
    Time: O(1)
    Space: O(1)
    
    Example: x = 1, y = 4
    Output: 2
    
    Trick: Count bits in x ^ y
    """
    xor = x ^ y
    count = 0
    while xor:
        xor &= xor - 1
        count += 1
    return count

# Test
print(hamming_distance(1, 4))  # 2
```

### Problem 8: Sum of Two Integers (No + operator)

```python
def get_sum(a: int, b: int) -> int:
    """
    Add two integers without using + or -.
    
    Time: O(1)
    Space: O(1)
    
    Example: a = 1, b = 2
    Output: 3
    
    Trick: XOR for sum, AND for carry
    """
    mask = 0xFFFFFFFF
    
    while b != 0:
        # Sum without carry
        sum_without_carry = (a ^ b) & mask
        # Carry
        carry = ((a & b) << 1) & mask
        
        a = sum_without_carry
        b = carry
    
    # Handle negative in Python
    return a if a <= 0x7FFFFFFF else ~(a ^ mask)

# Test
print(get_sum(1, 2))   # 3
print(get_sum(-1, 1))  # 0
```

### Problem 9: Power of Four

```python
def is_power_of_four(n: int) -> bool:
    """
    Check if n is power of 4.
    
    Time: O(1)
    Space: O(1)
    
    Trick: Power of 2 + bit at even position
    """
    # Must be power of 2
    if n <= 0 or (n & (n - 1)) != 0:
        return False
    
    # Check if bit is at even position
    # 0x55555555 = 01010101... (bits at even positions)
    return (n & 0x55555555) != 0

# Test
print(is_power_of_four(16))  # True
print(is_power_of_four(8))   # False
```

### Problem 10: Maximum XOR of Two Numbers

```python
class TrieNode:
    def __init__(self):
        self.children = {}

def find_maximum_xor(nums: list[int]) -> int:
    """
    Find maximum XOR of two numbers.
    
    Time: O(n)
    Space: O(1)
    
    Example: nums = [3,10,5,25,2,8]
    Output: 28 (5 ^ 25 = 28)
    """
    root = TrieNode()
    
    # Build trie
    for num in nums:
        node = root
        for i in range(31, -1, -1):
            bit = (num >> i) & 1
            if bit not in node.children:
                node.children[bit] = TrieNode()
            node = node.children[bit]
    
    max_xor = 0
    
    # Find max XOR for each number
    for num in nums:
        node = root
        current_xor = 0
        
        for i in range(31, -1, -1):
            bit = (num >> i) & 1
            # Try opposite bit for max XOR
            opposite = 1 - bit
            
            if opposite in node.children:
                current_xor |= (1 << i)
                node = node.children[opposite]
            else:
                node = node.children[bit]
        
        max_xor = max(max_xor, current_xor)
    
    return max_xor

# Test
print(find_maximum_xor([3,10,5,25,2,8]))  # 28
```

### Problem 11: Subsets (Bit Manipulation)

```python
def subsets(nums: list[int]) -> list[list[int]]:
    """
    Generate all subsets using bit manipulation.
    
    Time: O(n * 2^n)
    Space: O(1) excluding output
    
    Example: nums = [1,2,3]
    Output: [[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]
    """
    n = len(nums)
    result = []
    
    # 2^n subsets
    for i in range(1 << n):
        subset = []
        for j in range(n):
            # Check if jth bit is set
            if i & (1 << j):
                subset.append(nums[j])
        result.append(subset)
    
    return result

# Test
print(subsets([1,2,3]))
```

---

## Interview Tips

### 1. Common Bit Patterns

```python
# Check if ith bit is set
if num & (1 << i):
    pass

# Set ith bit
num |= (1 << i)

# Clear ith bit
num &= ~(1 << i)

# Toggle ith bit
num ^= (1 << i)

# Get rightmost 1 bit
rightmost = num & -num

# Remove rightmost 1 bit
num &= num - 1

# Check power of 2
is_power = num > 0 and (num & (num - 1)) == 0
```

### 2. XOR Tricks

```python
# Find unique element (others appear twice)
result = 0
for num in nums:
    result ^= num

# Swap without temp
a ^= b
b ^= a
a ^= b

# Check if two numbers have opposite signs
((a ^ b) < 0)
```

### 3. Shift Operations

```python
# Multiply by 2^k
num << k

# Divide by 2^k
num >> k

# Check if odd/even
is_odd = num & 1

# Get last k bits
last_k_bits = num & ((1 << k) - 1)
```

### 4. Common Mistakes

```python
# ❌ Forgetting operator precedence
if n & 1 == 0:  # Wrong! == has higher precedence
    pass

# ✅ Use parentheses
if (n & 1) == 0:
    pass

# ❌ Not handling negative numbers
# Python has unlimited precision
# May need masking for 32-bit

# ✅ Use mask
mask = 0xFFFFFFFF
result = num & mask
```

---

## Practice Problems

### Easy
1. [Number of 1 Bits](https://leetcode.com/problems/number-of-1-bits/)
2. [Power of Two](https://leetcode.com/problems/power-of-two/)
3. [Reverse Bits](https://leetcode.com/problems/reverse-bits/)
4. [Missing Number](https://leetcode.com/problems/missing-number/)
5. [Hamming Distance](https://leetcode.com/problems/hamming-distance/)

### Medium
1. [Single Number](https://leetcode.com/problems/single-number/)
2. [Single Number II](https://leetcode.com/problems/single-number-ii/)
3. [Single Number III](https://leetcode.com/problems/single-number-iii/)
4. [Sum of Two Integers](https://leetcode.com/problems/sum-of-two-integers/)
5. [Counting Bits](https://leetcode.com/problems/counting-bits/)
6. [Maximum XOR of Two Numbers](https://leetcode.com/problems/maximum-xor-of-two-numbers-in-an-array/)

### Hard
1. [Maximum XOR With an Element From Array](https://leetcode.com/problems/maximum-xor-with-an-element-from-array/)

---

## Summary

### Key Takeaways
- ✅ **XOR properties** - Self-cancel, commutative
- ✅ **Common tricks** - Power of 2, count bits, swap
- ✅ **Very fast** - O(1) operations
- ✅ **Space efficient** - Bit sets

### Quick Reference

```python
# Basic Operations
n & (n-1)         # Remove rightmost 1
n & -n            # Isolate rightmost 1
n | (1 << i)      # Set ith bit
n & ~(1 << i)     # Clear ith bit
n ^ (1 << i)      # Toggle ith bit
(n >> i) & 1      # Get ith bit

# Common Checks
n > 0 and (n & (n-1)) == 0  # Power of 2
n & 1                        # Odd/even
bin(n).count('1')            # Count 1s

# XOR Properties
a ^ a = 0
a ^ 0 = a
a ^ b ^ b = a
```

---

**Next**: [Math & Geometry →](../09-math-geometry/README.md)

**Happy Coding! 🚀**
