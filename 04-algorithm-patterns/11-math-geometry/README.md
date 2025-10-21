# 🔢 Math & Geometry - Python DSA

> Master mathematical algorithms and geometric problem-solving

---

## 📚 Table of Contents

1. [Introduction](#introduction)
2. [Number Theory](#number-theory)
3. [Geometric Algorithms](#geometric-algorithms)
4. [Matrix Operations](#matrix-operations)
5. [Mathematical Problems](#mathematical-problems)
6. [Interview Tips](#interview-tips)
7. [Practice Problems](#practice-problems)

---

## Introduction

**Math & Geometry** covers mathematical algorithms, number theory, and geometric computations.

### Key Characteristics
- ✅ **Mathematical formulas** - Direct solutions
- ✅ **Pattern recognition** - Mathematical patterns
- ✅ **Optimization** - Efficient algorithms
- ✅ **Edge cases** - Handle special cases

### When to Use
- **Prime** numbers and factorization
- **GCD/LCM** problems
- **Geometric** computations
- **Matrix** transformations
- Keywords: "prime", "divisor", "geometry", "rotate", "spiral"

---

## Number Theory

### Problem 1: Sieve of Eratosthenes (Find Primes)

```python
def count_primes(n: int) -> int:
    """
    Count prime numbers less than n.
    
    Time: O(n log log n)
    Space: O(n)
    
    Example: n = 10
    Output: 4 (primes: 2, 3, 5, 7)
    
    Algorithm: Sieve of Eratosthenes
    """
    if n <= 2:
        return 0
    
    # Initialize all as prime
    is_prime = [True] * n
    is_prime[0] = is_prime[1] = False
    
    # Mark multiples as not prime
    for i in range(2, int(n**0.5) + 1):
        if is_prime[i]:
            # Mark all multiples
            for j in range(i * i, n, i):
                is_prime[j] = False
    
    return sum(is_prime)

def get_primes(n: int) -> list[int]:
    """Get all primes less than n."""
    if n <= 2:
        return []
    
    is_prime = [True] * n
    is_prime[0] = is_prime[1] = False
    
    for i in range(2, int(n**0.5) + 1):
        if is_prime[i]:
            for j in range(i * i, n, i):
                is_prime[j] = False
    
    return [i for i in range(n) if is_prime[i]]
```

### Problem 2: GCD and LCM

```python
def gcd(a: int, b: int) -> int:
    """
    Greatest Common Divisor (Euclidean Algorithm).
    
    Time: O(log min(a,b))
    Space: O(1)
    
    Example: gcd(48, 18) = 6
    """
    while b:
        a, b = b, a % b
    return a

def lcm(a: int, b: int) -> int:
    """
    Least Common Multiple.
    
    Formula: LCM(a,b) = (a * b) / GCD(a,b)
    """
    return (a * b) // gcd(a, b)

# Using Python's built-in (Python 3.9+)
import math
result = math.gcd(48, 18)  # 6
result = math.lcm(48, 18)  # 144
```

### Problem 3: Prime Factorization

```python
def prime_factors(n: int) -> list[int]:
    """
    Find all prime factors.
    
    Time: O(√n)
    Space: O(log n)
    
    Example: n = 60
    Output: [2, 2, 3, 5] (60 = 2² × 3 × 5)
    """
    factors = []
    
    # Check for 2s
    while n % 2 == 0:
        factors.append(2)
        n //= 2
    
    # Check odd factors
    i = 3
    while i * i <= n:
        while n % i == 0:
            factors.append(i)
            n //= i
        i += 2
    
    # If n is prime > 2
    if n > 2:
        factors.append(n)
    
    return factors

def count_divisors(n: int) -> int:
    """
    Count number of divisors.
    
    Time: O(√n)
    Space: O(1)
    
    Example: n = 12
    Output: 6 (divisors: 1,2,3,4,6,12)
    """
    count = 0
    i = 1
    while i * i <= n:
        if n % i == 0:
            if i * i == n:
                count += 1
            else:
                count += 2
        i += 1
    return count
```

### Problem 4: Modular Arithmetic

```python
def power_mod(base: int, exp: int, mod: int) -> int:
    """
    Compute (base^exp) % mod efficiently.
    
    Time: O(log exp)
    Space: O(1)
    
    Example: power_mod(2, 10, 1000) = 24
    """
    result = 1
    base = base % mod
    
    while exp > 0:
        if exp % 2 == 1:
            result = (result * base) % mod
        exp //= 2
        base = (base * base) % mod
    
    return result

# Python built-in
result = pow(2, 10, 1000)  # 24

def factorial_mod(n: int, mod: int) -> int:
    """
    Compute n! % mod.
    
    Time: O(n)
    Space: O(1)
    """
    result = 1
    for i in range(1, n + 1):
        result = (result * i) % mod
    return result
```

### Problem 5: Combinations and Permutations

```python
def factorial(n: int) -> int:
    """Calculate n! iteratively."""
    result = 1
    for i in range(1, n + 1):
        result *= i
    return result

def permutation(n: int, r: int) -> int:
    """
    Calculate P(n,r) = n! / (n-r)!
    
    Example: P(5,3) = 60
    """
    result = 1
    for i in range(n, n - r, -1):
        result *= i
    return result

def combination(n: int, r: int) -> int:
    """
    Calculate C(n,r) = n! / (r! × (n-r)!)
    
    Example: C(5,3) = 10
    """
    if r > n - r:
        r = n - r
    
    result = 1
    for i in range(r):
        result = result * (n - i) // (i + 1)
    
    return result

# Python built-in (Python 3.8+)
import math
result = math.comb(5, 3)  # 10
result = math.perm(5, 3)  # 60
```

---

## Geometric Algorithms

### Problem 6: Rotate Image (Matrix 90°)

```python
def rotate(matrix: list[list[int]]) -> None:
    """
    Rotate matrix 90° clockwise in-place.
    
    Time: O(n²)
    Space: O(1)
    
    Example:
    [1,2,3]    [7,4,1]
    [4,5,6] -> [8,5,2]
    [7,8,9]    [9,6,3]
    
    Algorithm:
    1. Transpose (swap matrix[i][j] with matrix[j][i])
    2. Reverse each row
    """
    n = len(matrix)
    
    # Transpose
    for i in range(n):
        for j in range(i + 1, n):
            matrix[i][j], matrix[j][i] = matrix[j][i], matrix[i][j]
    
    # Reverse each row
    for i in range(n):
        matrix[i].reverse()

def rotate_counter_clockwise(matrix: list[list[int]]) -> None:
    """
    Rotate 90° counter-clockwise.
    
    Algorithm:
    1. Transpose
    2. Reverse each column (or reverse rows first, then transpose)
    """
    n = len(matrix)
    
    # Reverse each row first
    for i in range(n):
        matrix[i].reverse()
    
    # Transpose
    for i in range(n):
        for j in range(i + 1, n):
            matrix[i][j], matrix[j][i] = matrix[j][i], matrix[i][j]
```

### Problem 7: Spiral Matrix

```python
def spiral_order(matrix: list[list[int]]) -> list[int]:
    """
    Return elements in spiral order.
    
    Time: O(m × n)
    Space: O(1)
    
    Example:
    [1,2,3]
    [4,5,6]  -> [1,2,3,6,9,8,7,4,5]
    [7,8,9]
    """
    if not matrix:
        return []
    
    result = []
    top, bottom = 0, len(matrix) - 1
    left, right = 0, len(matrix[0]) - 1
    
    while top <= bottom and left <= right:
        # Traverse right
        for col in range(left, right + 1):
            result.append(matrix[top][col])
        top += 1
        
        # Traverse down
        for row in range(top, bottom + 1):
            result.append(matrix[row][right])
        right -= 1
        
        # Traverse left (if still have rows)
        if top <= bottom:
            for col in range(right, left - 1, -1):
                result.append(matrix[bottom][col])
            bottom -= 1
        
        # Traverse up (if still have columns)
        if left <= right:
            for row in range(bottom, top - 1, -1):
                result.append(matrix[row][left])
            left += 1
    
    return result

def generate_matrix(n: int) -> list[list[int]]:
    """
    Generate n×n spiral matrix with numbers 1 to n².
    
    Example: n = 3
    Output:
    [1,2,3]
    [8,9,4]
    [7,6,5]
    """
    matrix = [[0] * n for _ in range(n)]
    num = 1
    top, bottom = 0, n - 1
    left, right = 0, n - 1
    
    while num <= n * n:
        # Right
        for col in range(left, right + 1):
            matrix[top][col] = num
            num += 1
        top += 1
        
        # Down
        for row in range(top, bottom + 1):
            matrix[row][right] = num
            num += 1
        right -= 1
        
        # Left
        for col in range(right, left - 1, -1):
            matrix[bottom][col] = num
            num += 1
        bottom -= 1
        
        # Up
        for row in range(bottom, top - 1, -1):
            matrix[row][left] = num
            num += 1
        left += 1
    
    return matrix
```

### Problem 8: Valid Square

```python
def valid_square(p1: list[int], p2: list[int], p3: list[int], p4: list[int]) -> bool:
    """
    Check if 4 points form a valid square.
    
    Time: O(1)
    Space: O(1)
    
    Algorithm: Square has 4 equal sides and 2 equal diagonals
    """
    def distance(p1, p2):
        return (p1[0] - p2[0])**2 + (p1[1] - p2[1])**2
    
    points = [p1, p2, p3, p4]
    distances = []
    
    # Calculate all pairwise distances
    for i in range(4):
        for j in range(i + 1, 4):
            distances.append(distance(points[i], points[j]))
    
    distances.sort()
    
    # Should have 4 equal sides and 2 equal diagonals
    return (distances[0] > 0 and
            distances[0] == distances[1] == distances[2] == distances[3] and
            distances[4] == distances[5])
```

---

## Matrix Operations

### Problem 9: Set Matrix Zeroes

```python
def set_zeroes(matrix: list[list[int]]) -> None:
    """
    Set entire row and column to 0 if element is 0.
    
    Time: O(m × n)
    Space: O(1)
    
    Example:
    [1,1,1]    [1,0,1]
    [1,0,1] -> [0,0,0]
    [1,1,1]    [1,0,1]
    
    Algorithm: Use first row and column as markers
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
    
    # Set zeros based on markers
    for i in range(1, m):
        for j in range(1, n):
            if matrix[i][0] == 0 or matrix[0][j] == 0:
                matrix[i][j] = 0
    
    # Handle first row
    if first_row_zero:
        for j in range(n):
            matrix[0][j] = 0
    
    # Handle first column
    if first_col_zero:
        for i in range(m):
            matrix[i][0] = 0
```

### Problem 10: Search 2D Matrix

```python
def search_matrix(matrix: list[list[int]], target: int) -> bool:
    """
    Search in row-wise and column-wise sorted matrix.
    
    Time: O(m + n)
    Space: O(1)
    
    Example:
    [1,  4,  7, 11]
    [2,  5,  8, 12]
    [3,  6,  9, 16]
    target = 5 -> True
    
    Algorithm: Start from top-right, move left or down
    """
    if not matrix or not matrix[0]:
        return False
    
    m, n = len(matrix), len(matrix[0])
    row, col = 0, n - 1
    
    while row < m and col >= 0:
        if matrix[row][col] == target:
            return True
        elif matrix[row][col] > target:
            col -= 1
        else:
            row += 1
    
    return False

def search_sorted_matrix(matrix: list[list[int]], target: int) -> bool:
    """
    Search in matrix where first integer of each row > last of previous.
    
    Time: O(log(m×n))
    Space: O(1)
    
    Algorithm: Binary search treating 2D as 1D
    """
    if not matrix or not matrix[0]:
        return False
    
    m, n = len(matrix), len(matrix[0])
    left, right = 0, m * n - 1
    
    while left <= right:
        mid = (left + right) // 2
        mid_val = matrix[mid // n][mid % n]
        
        if mid_val == target:
            return True
        elif mid_val < target:
            left = mid + 1
        else:
            right = mid - 1
    
    return False
```

---

## Mathematical Problems

### Problem 11: Happy Number

```python
def is_happy(n: int) -> bool:
    """
    Check if number is happy.
    
    Time: O(log n)
    Space: O(log n)
    
    Example: n = 19
    Output: True (1²+9²=82, 8²+2²=68, ..., 1)
    
    Algorithm: Use set to detect cycle
    """
    seen = set()
    
    while n != 1 and n not in seen:
        seen.add(n)
        n = sum(int(digit)**2 for digit in str(n))
    
    return n == 1
```

### Problem 12: Plus One

```python
def plus_one(digits: list[int]) -> list[int]:
    """
    Add 1 to number represented as array.
    
    Time: O(n)
    Space: O(1)
    
    Example: [1,2,3] -> [1,2,4]
    Example: [9,9,9] -> [1,0,0,0]
    """
    n = len(digits)
    
    for i in range(n - 1, -1, -1):
        if digits[i] < 9:
            digits[i] += 1
            return digits
        digits[i] = 0
    
    # All were 9s
    return [1] + digits
```

### Problem 13: Pow(x, n)

```python
def my_pow(x: float, n: int) -> float:
    """
    Calculate x^n.
    
    Time: O(log n)
    Space: O(1)
    
    Example: x = 2.0, n = 10
    Output: 1024.0
    
    Algorithm: Fast exponentiation
    """
    if n == 0:
        return 1
    
    if n < 0:
        x = 1 / x
        n = -n
    
    result = 1
    current = x
    
    while n > 0:
        if n % 2 == 1:
            result *= current
        current *= current
        n //= 2
    
    return result
```

### Problem 14: Sqrt(x)

```python
def my_sqrt(x: int) -> int:
    """
    Calculate square root (floor).
    
    Time: O(log x)
    Space: O(1)
    
    Example: x = 8
    Output: 2 (√8 = 2.828...)
    
    Algorithm: Binary search
    """
    if x < 2:
        return x
    
    left, right = 1, x // 2
    
    while left <= right:
        mid = (left + right) // 2
        if mid * mid == x:
            return mid
        elif mid * mid < x:
            left = mid + 1
        else:
            right = mid - 1
    
    return right
```

### Problem 15: Missing Number

```python
def missing_number(nums: list[int]) -> int:
    """
    Find missing number in [0,n].
    
    Time: O(n)
    Space: O(1)
    
    Example: [3,0,1] -> 2
    
    Multiple approaches:
    """
    n = len(nums)
    
    # Method 1: Mathematical formula
    expected_sum = n * (n + 1) // 2
    actual_sum = sum(nums)
    return expected_sum - actual_sum
    
    # Method 2: XOR
    result = n
    for i, num in enumerate(nums):
        result ^= i ^ num
    return result
```

---

## Interview Tips

### 1. Prime Number Patterns

```python
# Check if prime
def is_prime(n):
    if n < 2:
        return False
    if n == 2:
        return True
    if n % 2 == 0:
        return False
    for i in range(3, int(n**0.5) + 1, 2):
        if n % i == 0:
            return False
    return True

# Sieve of Eratosthenes
def sieve(n):
    is_prime = [True] * n
    is_prime[0] = is_prime[1] = False
    for i in range(2, int(n**0.5) + 1):
        if is_prime[i]:
            for j in range(i*i, n, i):
                is_prime[j] = False
    return [i for i in range(n) if is_prime[i]]
```

### 2. GCD and LCM

```python
# Euclidean Algorithm
def gcd(a, b):
    while b:
        a, b = b, a % b
    return a

# LCM formula
def lcm(a, b):
    return (a * b) // gcd(a, b)
```

### 3. Matrix Rotation

```python
# 90° clockwise: Transpose + Reverse rows
def rotate_90(matrix):
    n = len(matrix)
    # Transpose
    for i in range(n):
        for j in range(i+1, n):
            matrix[i][j], matrix[j][i] = matrix[j][i], matrix[i][j]
    # Reverse rows
    for row in matrix:
        row.reverse()

# 90° counter-clockwise: Reverse rows + Transpose
def rotate_90_ccw(matrix):
    for row in matrix:
        row.reverse()
    n = len(matrix)
    for i in range(n):
        for j in range(i+1, n):
            matrix[i][j], matrix[j][i] = matrix[j][i], matrix[i][j]
```

### 4. Fast Exponentiation

```python
def power(base, exp, mod=None):
    result = 1
    base = base % mod if mod else base
    
    while exp > 0:
        if exp % 2 == 1:
            result = (result * base) % mod if mod else result * base
        exp //= 2
        base = (base * base) % mod if mod else base * base
    
    return result
```

---

## Practice Problems

### Easy
1. [Count Primes](https://leetcode.com/problems/count-primes/)
2. [Happy Number](https://leetcode.com/problems/happy-number/)
3. [Plus One](https://leetcode.com/problems/plus-one/)
4. [Missing Number](https://leetcode.com/problems/missing-number/)
5. [Sqrt(x)](https://leetcode.com/problems/sqrtx/)

### Medium
1. [Rotate Image](https://leetcode.com/problems/rotate-image/)
2. [Spiral Matrix](https://leetcode.com/problems/spiral-matrix/)
3. [Spiral Matrix II](https://leetcode.com/problems/spiral-matrix-ii/)
4. [Set Matrix Zeroes](https://leetcode.com/problems/set-matrix-zeroes/)
5. [Search a 2D Matrix II](https://leetcode.com/problems/search-a-2d-matrix-ii/)
6. [Pow(x, n)](https://leetcode.com/problems/powx-n/)
7. [Valid Square](https://leetcode.com/problems/valid-square/)

### Hard
1. [Rectangle Area II](https://leetcode.com/problems/rectangle-area-ii/)
2. [Max Points on a Line](https://leetcode.com/problems/max-points-on-a-line/)

---

## Summary

### Key Takeaways
- ✅ **Primes** - Sieve of Eratosthenes O(n log log n)
- ✅ **GCD** - Euclidean algorithm O(log min(a,b))
- ✅ **Matrix rotation** - Transpose + reverse
- ✅ **Fast exponentiation** - O(log n)

### Quick Reference

```python
# Primes (Sieve)
is_prime = [True] * n
for i in range(2, int(n**0.5) + 1):
    if is_prime[i]:
        for j in range(i*i, n, i):
            is_prime[j] = False

# GCD
def gcd(a, b):
    while b:
        a, b = b, a % b
    return a

# Rotate Matrix 90° Clockwise
# 1. Transpose
for i in range(n):
    for j in range(i+1, n):
        matrix[i][j], matrix[j][i] = matrix[j][i], matrix[i][j]
# 2. Reverse rows
for row in matrix:
    row.reverse()

# Fast Power
result = 1
while exp > 0:
    if exp % 2 == 1:
        result *= base
    base *= base
    exp //= 2
```

---

**Next**: [Monotonic Stack →](../12-monotonic-stack/README.md)

**Happy Coding! 🚀**
