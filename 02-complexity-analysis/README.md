# ⏱️ Complexity Analysis

> Understanding Time and Space Complexity - The Foundation of Algorithm Analysis

---

## 📚 Table of Contents

1. [Why Complexity Analysis?](#why-complexity-analysis)
2. [Time Complexity](#time-complexity)
3. [Space Complexity](#space-complexity)
4. [Big O Notation](#big-o-notation)
5. [Common Time Complexities](#common-time-complexities)
6. [How to Calculate Complexity](#how-to-calculate-complexity)
7. [Amortized Analysis](#amortized-analysis)
8. [Practice Problems](#practice-problems)

---

## Why Complexity Analysis?

Complexity analysis helps us:
- **Compare algorithms** objectively
- **Predict performance** for large inputs
- **Optimize code** systematically
- **Ace interviews** by discussing trade-offs

### Real-World Example

```python
# Which is better for finding duplicates?

# Approach 1: Nested loops
def has_duplicates_v1(nums):
    for i in range(len(nums)):
        for j in range(i + 1, len(nums)):
            if nums[i] == nums[j]:
                return True
    return False

# Approach 2: Using set
def has_duplicates_v2(nums):
    return len(nums) != len(set(nums))

# For nums = [1, 2, 3, ..., 10000]:
# v1: ~50 million comparisons (O(n²))
# v2: ~10,000 operations (O(n))
```

---

## Time Complexity

**Time complexity** measures how the runtime grows as input size increases.

### Key Points
- Focus on **growth rate**, not exact time
- Consider **worst-case** unless stated otherwise
- Ignore **constants** and **lower-order terms**

### Example

```python
def example(n):
    total = 0           # 1 operation
    
    for i in range(n):  # n iterations
        total += i      # 1 operation per iteration
    
    return total        # 1 operation

# Total: 1 + n + 1 = n + 2 operations
# Time Complexity: O(n) - we drop constants
```

---

## Space Complexity

**Space complexity** measures extra memory used as input size increases.

### Components
1. **Input space**: Memory for input (usually not counted)
2. **Auxiliary space**: Extra memory used by algorithm
3. **Output space**: Memory for output (sometimes counted)

### Example

```python
def create_matrix(n):
    # Creates n x n matrix
    matrix = [[0] * n for _ in range(n)]
    return matrix

# Space Complexity: O(n²) - stores n² elements
```

---

## Big O Notation

Big O describes the **upper bound** (worst case) of algorithm growth.

### Asymptotic Notations

| Notation | Meaning | Use Case |
|----------|---------|----------|
| **O** (Big O) | Upper bound (≤) | Worst case |
| **Ω** (Omega) | Lower bound (≥) | Best case |
| **Θ** (Theta) | Tight bound (=) | Average case |

### Big O Rules

1. **Drop constants**: O(2n) → O(n)
2. **Drop lower terms**: O(n² + n) → O(n²)
3. **Different inputs**: O(a + b) or O(a * b)
4. **Nested loops**: Often multiply complexities

```python
# Rule 1: Drop constants
def print_twice(n):
    for i in range(n):
        print(i)
    for i in range(n):
        print(i)
# O(n + n) = O(2n) = O(n)

# Rule 2: Drop lower terms
def mixed(n):
    for i in range(n):        # O(n)
        for j in range(n):    # O(n)
            print(i, j)
    for i in range(n):        # O(n)
        print(i)
# O(n² + n) = O(n²)

# Rule 3: Different inputs
def combine(a, b):
    for x in a:               # O(len(a))
        print(x)
    for y in b:               # O(len(b))
        print(y)
# O(len(a) + len(b)) = O(a + b)

# Rule 4: Nested loops
def nested(n):
    for i in range(n):        # O(n)
        for j in range(n):    # O(n) for each i
            print(i, j)
# O(n * n) = O(n²)
```

---

## Common Time Complexities

### From Best to Worst

| Complexity | Name | Example | When n=1000 |
|------------|------|---------|-------------|
| **O(1)** | Constant | Array access, hash lookup | 1 |
| **O(log n)** | Logarithmic | Binary search | ~10 |
| **O(n)** | Linear | Array traversal | 1,000 |
| **O(n log n)** | Linearithmic | Merge sort, heap sort | ~10,000 |
| **O(n²)** | Quadratic | Nested loops | 1,000,000 |
| **O(n³)** | Cubic | Triple nested loops | 1,000,000,000 |
| **O(2ⁿ)** | Exponential | Fibonacci (naive) | 10³⁰⁰ |
| **O(n!)** | Factorial | Permutations | Astronomical |

### Complexity Graph

```
O(1) ────────────────────────────
O(log n) ──────────╱
O(n)     ─────────╱
O(n log n) ──────╱
O(n²)    ───────╱
O(2ⁿ)    ──────╱
         │
         └─────────────> Input Size (n)
```

---

## How to Calculate Complexity

### Step-by-Step Process

1. **Identify basic operations** (comparisons, arithmetic, etc.)
2. **Count operations** as function of input size
3. **Express in Big O** notation (drop constants/lower terms)

### Examples

#### Example 1: Simple Loop

```python
def sum_array(arr):
    total = 0                    # O(1)
    for num in arr:              # O(n)
        total += num             # O(1) per iteration
    return total                 # O(1)

# Time: O(1) + O(n) * O(1) + O(1) = O(n)
# Space: O(1) - only using 'total' variable
```

#### Example 2: Nested Loops

```python
def print_pairs(arr):
    for i in range(len(arr)):         # O(n)
        for j in range(len(arr)):     # O(n) for each i
            print(arr[i], arr[j])     # O(1)

# Time: O(n) * O(n) * O(1) = O(n²)
# Space: O(1) - no extra data structures
```

#### Example 3: Binary Search

```python
def binary_search(arr, target):
    left, right = 0, len(arr) - 1
    
    while left <= right:              # O(log n)
        mid = (left + right) // 2
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return -1

# Time: O(log n) - halves search space each iteration
# Space: O(1) - only using variables
```

#### Example 4: Recursive Fibonacci

```python
def fibonacci(n):
    if n <= 1:                        # O(1)
        return n
    return fibonacci(n-1) + fibonacci(n-2)

# Each call makes 2 more calls → Tree of depth n
# Time: O(2ⁿ) - exponential!
# Space: O(n) - recursion stack depth
```

#### Example 5: Two Separate Loops

```python
def process(arr1, arr2):
    # First loop
    for x in arr1:                    # O(a)
        print(x)
    
    # Second loop
    for y in arr2:                    # O(b)
        print(y)

# Time: O(a + b) - different inputs!
# NOT O(n) - we have TWO different arrays
```

#### Example 6: Nested with Different Loops

```python
def nested_different(arr1, arr2):
    for x in arr1:                    # O(a)
        for y in arr2:                # O(b) for each x
            print(x, y)

# Time: O(a * b) - multiplied!
```

---

## Common Patterns & Their Complexities

### 1. Single Loop: O(n)

```python
def linear_search(arr, target):
    for num in arr:
        if num == target:
            return True
    return False
```

### 2. Two Loops (Separate): O(a + b)

```python
def print_both(arr1, arr2):
    for x in arr1:
        print(x)
    for y in arr2:
        print(y)
```

### 3. Nested Loops: O(n²)

```python
def bubble_sort(arr):
    for i in range(len(arr)):
        for j in range(len(arr) - i - 1):
            if arr[j] > arr[j + 1]:
                arr[j], arr[j + 1] = arr[j + 1], arr[j]
```

### 4. Divide & Conquer: O(n log n)

```python
def merge_sort(arr):
    if len(arr) <= 1:
        return arr
    
    mid = len(arr) // 2
    left = merge_sort(arr[:mid])      # Divide
    right = merge_sort(arr[mid:])     # Divide
    return merge(left, right)         # Conquer: O(n)

# Recurrence: T(n) = 2T(n/2) + O(n) = O(n log n)
```

### 5. Halving Input: O(log n)

```python
def count_bits(n):
    count = 0
    while n > 0:
        count += 1
        n //= 2  # Halve n each time
    return count

# n → n/2 → n/4 → ... → 1
# Takes log₂(n) steps
```

### 6. All Subsets: O(2ⁿ)

```python
def generate_subsets(arr):
    result = []
    
    def backtrack(start, path):
        result.append(path[:])
        for i in range(start, len(arr)):
            path.append(arr[i])
            backtrack(i + 1, path)
            path.pop()
    
    backtrack(0, [])
    return result

# Each element: include or exclude → 2ⁿ subsets
```

### 7. All Permutations: O(n!)

```python
def permutations(arr):
    if len(arr) <= 1:
        return [arr]
    
    result = []
    for i in range(len(arr)):
        rest = arr[:i] + arr[i+1:]
        for p in permutations(rest):
            result.append([arr[i]] + p)
    return result

# n choices × (n-1) × (n-2) × ... × 1 = n!
```

---

## Space Complexity Patterns

### O(1) - Constant Space

```python
def swap(arr, i, j):
    arr[i], arr[j] = arr[j], arr[i]

# Only uses temp variables
```

### O(n) - Linear Space

```python
def copy_array(arr):
    return arr[:]  # Creates new array

# Using hash set
def unique_elements(arr):
    seen = set()  # Could be size n
    for num in arr:
        seen.add(num)
    return len(seen)
```

### O(n) - Recursion Stack

```python
def factorial(n):
    if n <= 1:
        return 1
    return n * factorial(n - 1)

# Recursion depth = n
# Space for call stack: O(n)
```

### O(n²) - 2D Array

```python
def create_matrix(n):
    return [[0] * n for _ in range(n)]

# Stores n × n elements
```

---

## Amortized Analysis

**Amortized analysis** considers the average cost over a sequence of operations.

### Example: Dynamic Array (Python list)

```python
# Python list append
arr = []
for i in range(n):
    arr.append(i)  # Usually O(1), occasionally O(n)

# Individual operations vary, but average is O(1)
```

**How it works:**
1. Array starts with capacity 4
2. When full, double capacity (resize)
3. Resize is O(n), but happens rarely
4. Most appends are O(1)

**Amortized cost**: O(1) per append

### When to Use
- Dynamic arrays (append)
- Hash tables (insert with rehashing)
- Splay trees (self-adjusting)

---

## Tips for Interviews

### 1. Always State Assumptions

```python
"I'm assuming the array is sorted..."
"For the average case, this is O(n)..."
```

### 2. Consider All Cases

- **Best case**: Minimum operations
- **Average case**: Expected operations
- **Worst case**: Maximum operations (default)

### 3. Mention Trade-offs

```python
"We can optimize time from O(n²) to O(n) 
 by using O(n) extra space for a hash set."
```

### 4. Recognize Patterns

- **Sorting?** → O(n log n)
- **Nested loops over same data?** → O(n²)
- **Halving problem size?** → O(log n)
- **All subsets/permutations?** → Exponential

---

## Practice Problems

### Problem 1: Analyze Complexity

```python
def mystery(n):
    i = 1
    while i < n:
        i *= 2
    return i
```

<details>
<summary>Answer</summary>

**Time**: O(log n) - i doubles each iteration (1, 2, 4, 8, ...)

**Space**: O(1) - only using variable i
</details>

### Problem 2: Analyze Complexity

```python
def mystery2(arr):
    n = len(arr)
    for i in range(n):
        for j in range(i, n):
            print(arr[i], arr[j])
```

<details>
<summary>Answer</summary>

**Time**: O(n²)
- Outer loop: n iterations
- Inner loop: n-i iterations (decreases)
- Total: n + (n-1) + (n-2) + ... + 1 = n(n+1)/2 ≈ n²/2 = O(n²)

**Space**: O(1)
</details>

### Problem 3: Optimize This Code

```python
def has_pair_sum(arr, target):
    """Check if any two numbers sum to target."""
    for i in range(len(arr)):
        for j in range(i + 1, len(arr)):
            if arr[i] + arr[j] == target:
                return True
    return False

# Current: O(n²) time, O(1) space
# Can you optimize to O(n) time?
```

<details>
<summary>Solution</summary>

```python
def has_pair_sum_optimized(arr, target):
    """Optimized using hash set."""
    seen = set()
    for num in arr:
        complement = target - num
        if complement in seen:
            return True
        seen.add(num)
    return False

# Time: O(n), Space: O(n)
# Trade-off: Use extra space for better time
```
</details>

---

## Complexity Cheat Sheet

### Data Structure Operations

| Data Structure | Access | Search | Insert | Delete | Space |
|---------------|--------|--------|--------|--------|-------|
| **Array** | O(1) | O(n) | O(n) | O(n) | O(n) |
| **Linked List** | O(n) | O(n) | O(1)* | O(1)* | O(n) |
| **Stack** | O(n) | O(n) | O(1) | O(1) | O(n) |
| **Queue** | O(n) | O(n) | O(1) | O(1) | O(n) |
| **Hash Table** | - | O(1)† | O(1)† | O(1)† | O(n) |
| **Binary Search Tree** | O(log n)† | O(log n)† | O(log n)† | O(log n)† | O(n) |
| **Heap** | O(1) | O(n) | O(log n) | O(log n) | O(n) |

*With pointer  
†Average case  

### Sorting Algorithms

| Algorithm | Time (Best) | Time (Avg) | Time (Worst) | Space |
|-----------|-------------|------------|--------------|-------|
| **Bubble Sort** | O(n) | O(n²) | O(n²) | O(1) |
| **Selection Sort** | O(n²) | O(n²) | O(n²) | O(1) |
| **Insertion Sort** | O(n) | O(n²) | O(n²) | O(1) |
| **Merge Sort** | O(n log n) | O(n log n) | O(n log n) | O(n) |
| **Quick Sort** | O(n log n) | O(n log n) | O(n²) | O(log n) |
| **Heap Sort** | O(n log n) | O(n log n) | O(n log n) | O(1) |

---

## 📚 Next Steps

Now that you understand complexity:
1. Learn [Data Structures](../03-data-structures/README.md)
2. Practice analyzing algorithms you encounter
3. Always think: "Can I do better?"

---

**Remember**: In interviews, it's not just about solving the problem—it's about finding the most efficient solution! 🚀
