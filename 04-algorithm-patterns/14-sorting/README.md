# 🔄 Sorting Algorithms - Python DSA

> Master all major sorting algorithms with implementations and comparisons

---

## 📚 Table of Contents

1. [Introduction](#introduction)
2. [Comparison Sorts](#comparison-sorts)
3. [Non-Comparison Sorts](#non-comparison-sorts)
4. [Algorithm Comparison](#algorithm-comparison)
5. [Interview Tips](#interview-tips)
6. [Practice Problems](#practice-problems)

---

## Introduction

**Sorting** is fundamental to many algorithms. Understanding different sorting techniques is crucial.

### Complexity Overview

| Algorithm | Best | Average | Worst | Space | Stable |
|-----------|------|---------|-------|-------|--------|
| Bubble Sort | O(n) | O(n²) | O(n²) | O(1) | ✅ |
| Selection Sort | O(n²) | O(n²) | O(n²) | O(1) | ❌ |
| Insertion Sort | O(n) | O(n²) | O(n²) | O(1) | ✅ |
| Merge Sort | O(n log n) | O(n log n) | O(n log n) | O(n) | ✅ |
| Quick Sort | O(n log n) | O(n log n) | O(n²) | O(log n) | ❌ |
| Heap Sort | O(n log n) | O(n log n) | O(n log n) | O(1) | ❌ |
| Counting Sort | O(n+k) | O(n+k) | O(n+k) | O(k) | ✅ |
| Radix Sort | O(d(n+k)) | O(d(n+k)) | O(d(n+k)) | O(n+k) | ✅ |
| Bucket Sort | O(n+k) | O(n+k) | O(n²) | O(n) | ✅ |

---

## Comparison Sorts

### 1. Bubble Sort

```python
def bubble_sort(arr: list[int]) -> list[int]:
    """
    Bubble Sort - repeatedly swap adjacent elements if in wrong order.
    
    Time: O(n²) average/worst, O(n) best
    Space: O(1)
    Stable: Yes
    
    Example: [64,34,25,12,22,11,90]
    Output: [11,12,22,25,34,64,90]
    
    Use: Small datasets, nearly sorted data
    """
    n = len(arr)
    
    for i in range(n):
        swapped = False
        
        for j in range(0, n - i - 1):
            if arr[j] > arr[j + 1]:
                arr[j], arr[j + 1] = arr[j + 1], arr[j]
                swapped = True
        
        # Optimization: stop if no swaps
        if not swapped:
            break
    
    return arr
```

### 2. Selection Sort

```python
def selection_sort(arr: list[int]) -> list[int]:
    """
    Selection Sort - find minimum and place at beginning.
    
    Time: O(n²) all cases
    Space: O(1)
    Stable: No
    
    Example: [64,25,12,22,11]
    Output: [11,12,22,25,64]
    
    Use: Small datasets, minimizing swaps
    """
    n = len(arr)
    
    for i in range(n):
        # Find minimum in remaining unsorted array
        min_idx = i
        for j in range(i + 1, n):
            if arr[j] < arr[min_idx]:
                min_idx = j
        
        # Swap minimum with first unsorted element
        arr[i], arr[min_idx] = arr[min_idx], arr[i]
    
    return arr
```

### 3. Insertion Sort

```python
def insertion_sort(arr: list[int]) -> list[int]:
    """
    Insertion Sort - build sorted array one element at a time.
    
    Time: O(n²) average/worst, O(n) best
    Space: O(1)
    Stable: Yes
    
    Example: [12,11,13,5,6]
    Output: [5,6,11,12,13]
    
    Use: Small datasets, nearly sorted data, online sorting
    """
    for i in range(1, len(arr)):
        key = arr[i]
        j = i - 1
        
        # Move elements greater than key one position ahead
        while j >= 0 and arr[j] > key:
            arr[j + 1] = arr[j]
            j -= 1
        
        arr[j + 1] = key
    
    return arr
```

### 4. Merge Sort

```python
def merge_sort(arr: list[int]) -> list[int]:
    """
    Merge Sort - divide and conquer, merge sorted halves.
    
    Time: O(n log n) all cases
    Space: O(n)
    Stable: Yes
    
    Example: [38,27,43,3,9,82,10]
    Output: [3,9,10,27,38,43,82]
    
    Use: Large datasets, guaranteed O(n log n), stable sort needed
    """
    if len(arr) <= 1:
        return arr
    
    mid = len(arr) // 2
    left = merge_sort(arr[:mid])
    right = merge_sort(arr[mid:])
    
    return merge(left, right)

def merge(left: list[int], right: list[int]) -> list[int]:
    """Merge two sorted arrays."""
    result = []
    i = j = 0
    
    while i < len(left) and j < len(right):
        if left[i] <= right[j]:
            result.append(left[i])
            i += 1
        else:
            result.append(right[j])
            j += 1
    
    result.extend(left[i:])
    result.extend(right[j:])
    
    return result
```

### 5. Quick Sort

```python
def quick_sort(arr: list[int]) -> list[int]:
    """
    Quick Sort - partition around pivot, recursively sort.
    
    Time: O(n log n) average, O(n²) worst
    Space: O(log n) average
    Stable: No
    
    Example: [10,7,8,9,1,5]
    Output: [1,5,7,8,9,10]
    
    Use: Average case performance, in-place sorting
    """
    if len(arr) <= 1:
        return arr
    
    pivot = arr[len(arr) // 2]
    left = [x for x in arr if x < pivot]
    middle = [x for x in arr if x == pivot]
    right = [x for x in arr if x > pivot]
    
    return quick_sort(left) + middle + quick_sort(right)

def quick_sort_inplace(arr: list[int], low: int, high: int) -> None:
    """
    Quick Sort in-place using Lomuto partition.
    
    Time: O(n log n) average
    Space: O(log n)
    """
    if low < high:
        pi = partition(arr, low, high)
        quick_sort_inplace(arr, low, pi - 1)
        quick_sort_inplace(arr, pi + 1, high)

def partition(arr: list[int], low: int, high: int) -> int:
    """Lomuto partition scheme."""
    pivot = arr[high]
    i = low - 1
    
    for j in range(low, high):
        if arr[j] <= pivot:
            i += 1
            arr[i], arr[j] = arr[j], arr[i]
    
    arr[i + 1], arr[high] = arr[high], arr[i + 1]
    return i + 1
```

### 6. Heap Sort

```python
def heap_sort(arr: list[int]) -> list[int]:
    """
    Heap Sort - build max heap, extract max repeatedly.
    
    Time: O(n log n) all cases
    Space: O(1)
    Stable: No
    
    Example: [12,11,13,5,6,7]
    Output: [5,6,7,11,12,13]
    
    Use: Guaranteed O(n log n), O(1) space
    """
    n = len(arr)
    
    # Build max heap
    for i in range(n // 2 - 1, -1, -1):
        heapify(arr, n, i)
    
    # Extract elements from heap
    for i in range(n - 1, 0, -1):
        arr[0], arr[i] = arr[i], arr[0]
        heapify(arr, i, 0)
    
    return arr

def heapify(arr: list[int], n: int, i: int) -> None:
    """Heapify subtree rooted at index i."""
    largest = i
    left = 2 * i + 1
    right = 2 * i + 2
    
    if left < n and arr[left] > arr[largest]:
        largest = left
    
    if right < n and arr[right] > arr[largest]:
        largest = right
    
    if largest != i:
        arr[i], arr[largest] = arr[largest], arr[i]
        heapify(arr, n, largest)
```

---

## Non-Comparison Sorts

### 7. Counting Sort

```python
def counting_sort(arr: list[int]) -> list[int]:
    """
    Counting Sort - count occurrences, reconstruct sorted array.
    
    Time: O(n + k) where k is range of input
    Space: O(k)
    Stable: Yes
    
    Example: [1,4,1,2,7,5,2]
    Output: [1,1,2,2,4,5,7]
    
    Use: Small range of integers, non-negative integers
    """
    if not arr:
        return arr
    
    # Find range
    min_val, max_val = min(arr), max(arr)
    range_size = max_val - min_val + 1
    
    # Count occurrences
    count = [0] * range_size
    for num in arr:
        count[num - min_val] += 1
    
    # Cumulative count
    for i in range(1, range_size):
        count[i] += count[i - 1]
    
    # Build output array
    output = [0] * len(arr)
    for num in reversed(arr):
        output[count[num - min_val] - 1] = num
        count[num - min_val] -= 1
    
    return output
```

### 8. Radix Sort

```python
def radix_sort(arr: list[int]) -> list[int]:
    """
    Radix Sort - sort by each digit (LSD to MSD).
    
    Time: O(d × (n + k)) where d is number of digits
    Space: O(n + k)
    Stable: Yes
    
    Example: [170,45,75,90,802,24,2,66]
    Output: [2,24,45,66,75,90,170,802]
    
    Use: Large numbers, fixed number of digits
    """
    if not arr:
        return arr
    
    # Find maximum to determine number of digits
    max_num = max(arr)
    exp = 1
    
    while max_num // exp > 0:
        counting_sort_by_digit(arr, exp)
        exp *= 10
    
    return arr

def counting_sort_by_digit(arr: list[int], exp: int) -> None:
    """Counting sort for specific digit."""
    n = len(arr)
    output = [0] * n
    count = [0] * 10
    
    # Count occurrences
    for num in arr:
        index = (num // exp) % 10
        count[index] += 1
    
    # Cumulative count
    for i in range(1, 10):
        count[i] += count[i - 1]
    
    # Build output
    for num in reversed(arr):
        index = (num // exp) % 10
        output[count[index] - 1] = num
        count[index] -= 1
    
    # Copy to original
    for i in range(n):
        arr[i] = output[i]
```

### 9. Bucket Sort

```python
def bucket_sort(arr: list[float]) -> list[float]:
    """
    Bucket Sort - distribute into buckets, sort buckets.
    
    Time: O(n + k) average, O(n²) worst
    Space: O(n)
    Stable: Yes
    
    Example: [0.897, 0.565, 0.656, 0.123, 0.665, 0.343]
    Output: [0.123, 0.343, 0.565, 0.656, 0.665, 0.897]
    
    Use: Uniformly distributed data, floating points
    """
    if not arr:
        return arr
    
    # Create buckets
    n = len(arr)
    buckets = [[] for _ in range(n)]
    
    # Distribute into buckets
    for num in arr:
        index = int(n * num)
        buckets[index].append(num)
    
    # Sort each bucket and concatenate
    result = []
    for bucket in buckets:
        result.extend(sorted(bucket))
    
    return result
```

---

## Algorithm Comparison

### When to Use Each Sort

```python
def choose_sorting_algorithm(data_size, data_type, requirements):
    """
    Decision tree for choosing sorting algorithm.
    
    Factors:
    - Data size
    - Data type (integers, floats, objects)
    - Memory constraints
    - Stability requirement
    - Nearly sorted data
    """
    
    # Small data (< 50 elements)
    if data_size < 50:
        return "Insertion Sort"
    
    # Nearly sorted data
    if is_nearly_sorted(data):
        return "Insertion Sort or Bubble Sort"
    
    # Need stable sort
    if requirements.stable:
        return "Merge Sort or Counting Sort (if integers)"
    
    # Memory constrained
    if requirements.low_memory:
        return "Heap Sort or Quick Sort"
    
    # Integer data with small range
    if is_integer_small_range(data):
        return "Counting Sort or Radix Sort"
    
    # General purpose
    return "Quick Sort (average case) or Merge Sort (worst case)"
```

### Stability Examples

```python
# Stable sort preserves relative order of equal elements
# Example: Sort by name, then by age (stable sort preserves name order for same age)

students = [
    ("Alice", 20),
    ("Bob", 20),
    ("Charlie", 19)
]

# Stable sort by age:
# [("Charlie", 19), ("Alice", 20), ("Bob", 20)]
# Alice before Bob preserved

# Unstable sort might give:
# [("Charlie", 19), ("Bob", 20), ("Alice", 20)]
# Order of Alice and Bob changed
```

---

## Interview Tips

### 1. Quick Sort vs Merge Sort

```python
# Quick Sort:
# + Faster in practice (better cache performance)
# + In-place (O(log n) space)
# - Not stable
# - O(n²) worst case

# Merge Sort:
# + Guaranteed O(n log n)
# + Stable
# - Requires O(n) extra space
# - Slower in practice

# Choose Quick Sort for: Average case performance, space constraint
# Choose Merge Sort for: Worst case guarantee, stability needed
```

### 2. Python's Built-in Sort

```python
# Python uses Timsort (hybrid of Merge Sort and Insertion Sort)
# Time: O(n log n), Space: O(n), Stable: Yes

# sorted() - returns new sorted list
sorted_arr = sorted(arr)

# list.sort() - sorts in-place
arr.sort()

# Custom key
arr.sort(key=lambda x: x[1])  # Sort by second element

# Reverse
arr.sort(reverse=True)
```

### 3. Common Interview Questions

```python
# Q1: Sort array of 0s, 1s, 2s
def sort_colors(nums):
    # Dutch National Flag (Three-way partitioning)
    # O(n) time, O(1) space
    pass

# Q2: Kth largest element
def find_kth_largest(nums, k):
    # Use Quick Select or Min Heap
    # O(n) average using Quick Select
    pass

# Q3: Merge k sorted arrays
def merge_k_sorted(arrays):
    # Use Min Heap
    # O(n log k) time
    pass

# Q4: Sort nearly sorted array
def sort_nearly_sorted(arr, k):
    # Use Min Heap of size k+1
    # O(n log k) time
    pass
```

### 4. Sorting Tricks

```python
# Trick 1: Count inversions using Merge Sort
def count_inversions(arr):
    """Count pairs (i,j) where i<j and arr[i]>arr[j]"""
    # Modify merge sort to count
    pass

# Trick 2: Find median using Quick Select
def find_median(arr):
    """O(n) average time"""
    n = len(arr)
    return quick_select(arr, n // 2)

# Trick 3: Sort linked list using Merge Sort
def sort_list(head):
    """O(n log n) time, O(1) space for linked list"""
    # Use merge sort with O(1) space
    pass
```

---

## Practice Problems

### Easy
1. [Sort Colors](https://leetcode.com/problems/sort-colors/)
2. [Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array/)
3. [Sort Array By Parity](https://leetcode.com/problems/sort-array-by-parity/)
4. [Squares of Sorted Array](https://leetcode.com/problems/squares-of-a-sorted-array/)

### Medium
1. [Sort an Array](https://leetcode.com/problems/sort-an-array/)
2. [Kth Largest Element](https://leetcode.com/problems/kth-largest-element-in-an-array/)
3. [Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/)
4. [Sort List](https://leetcode.com/problems/sort-list/)
5. [Merge Intervals](https://leetcode.com/problems/merge-intervals/)
6. [Wiggle Sort II](https://leetcode.com/problems/wiggle-sort-ii/)

### Hard
1. [Count of Smaller Numbers After Self](https://leetcode.com/problems/count-of-smaller-numbers-after-self/)
2. [Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/)
3. [Maximum Gap](https://leetcode.com/problems/maximum-gap/)

---

## Summary

### Key Takeaways
- ✅ **O(n log n)** - Best comparison-based sorts
- ✅ **O(n)** - Non-comparison sorts for specific cases
- ✅ **Stability** - Preserves relative order
- ✅ **Space** - Trade-off between time and space

### Quick Reference

```python
# General Purpose
arr.sort()  # Python's Timsort O(n log n)

# Small Data
insertion_sort(arr)  # O(n²) but fast for small n

# Guaranteed Performance
merge_sort(arr)  # O(n log n) always

# Space Constrained
heap_sort(arr)  # O(n log n) time, O(1) space

# Integer Range Known
counting_sort(arr)  # O(n + k) linear time

# Stability Needed
merge_sort(arr) or arr.sort()  # Both stable

# In-place Sorting
quick_sort(arr) or heap_sort(arr)
```

---

**🎉 Algorithm Patterns Complete!**

**Happy Coding! 🚀**
