# 🔄 Backtracking - Python DSA

> Explore all possibilities through exhaustive search

---

## 📚 Table of Contents

1. [Introduction](#introduction)
2. [Core Template](#core-template)
3. [Combinations](#combinations)
4. [Permutations](#permutations)
5. [Subsets](#subsets)
6. [Constraint Satisfaction](#constraint-satisfaction)
7. [Interview Tips](#interview-tips)
8. [Practice Problems](#practice-problems)

---

## Introduction

**Backtracking** builds solutions incrementally and abandons candidates that fail constraints.

### Key Characteristics
- ✅ **Try all possibilities** - Exhaustive search
- ✅ **Prune invalid paths** - Stop early when constraints violated
- ✅ **Recursive structure** - Build solution step by step
- ✅ **Undo choices** - Backtrack when path fails

### When to Use
- Need **all possible solutions** or combinations
- Problem has **constraints** to satisfy
- Can **prune** search space early
- Keywords: "all combinations", "all permutations", "generate all"

### Backtracking vs Brute Force
```python
# Brute Force:
- Generate all solutions
- Check validity afterward
- Less efficient

# Backtracking:
- Check constraints during generation
- Prune invalid paths early
- More efficient
```

---

## Core Template

### General Backtracking Structure

```python
def backtrack(path, choices, result):
    """
    General backtracking template.
    
    Args:
        path: Current solution being built
        choices: Available choices at current step
        result: Collection of all valid solutions
    """
    # Base case: solution complete
    if is_valid_solution(path):
        result.append(path.copy())
        return
    
    # Try each available choice
    for choice in choices:
        # Make choice
        path.append(choice)
        
        # Recurse with updated choices
        backtrack(path, get_next_choices(choice), result)
        
        # Undo choice (backtrack)
        path.pop()
```

### Example: Generate Binary Strings

```python
def generate_binary(n: int) -> list[str]:
    """
    Generate all binary strings of length n.
    
    Time: O(2^n)
    Space: O(n)
    """
    result = []
    
    def backtrack(path):
        if len(path) == n:
            result.append(''.join(path))
            return
        
        for digit in ['0', '1']:
            path.append(digit)
            backtrack(path)
            path.pop()
    
    backtrack([])
    return result

# Test
print(generate_binary(3))
# ['000', '001', '010', '011', '100', '101', '110', '111']
```

---

## Combinations

### Problem 1: Combinations

```python
def combine(n: int, k: int) -> list[list[int]]:
    """
    All combinations of k numbers from 1 to n.
    
    Time: O(C(n,k))
    Space: O(k)
    
    Example: n = 4, k = 2
    Output: [[1,2],[1,3],[1,4],[2,3],[2,4],[3,4]]
    """
    result = []
    
    def backtrack(start, path):
        if len(path) == k:
            result.append(path[:])
            return
        
        for i in range(start, n + 1):
            path.append(i)
            backtrack(i + 1, path)
            path.pop()
    
    backtrack(1, [])
    return result

# Test
print(combine(4, 2))
```

### Problem 2: Combination Sum

```python
def combination_sum(candidates: list[int], target: int) -> list[list[int]]:
    """
    Find all combinations that sum to target (reuse allowed).
    
    Time: O(n^(target/min))
    Space: O(target/min)
    
    Example: candidates = [2,3,6,7], target = 7
    Output: [[2,2,3],[7]]
    """
    result = []
    candidates.sort()
    
    def backtrack(start, path, remaining):
        if remaining == 0:
            result.append(path[:])
            return
        
        if remaining < 0:
            return
        
        for i in range(start, len(candidates)):
            path.append(candidates[i])
            backtrack(i, path, remaining - candidates[i])  # i, not i+1 (reuse)
            path.pop()
    
    backtrack(0, [], target)
    return result

# Test
print(combination_sum([2,3,6,7], 7))  # [[2,2,3],[7]]
```

### Problem 3: Combination Sum II (No Reuse)

```python
def combination_sum2(candidates: list[int], target: int) -> list[list[int]]:
    """
    Find combinations that sum to target (no reuse, handle duplicates).
    
    Time: O(2^n)
    Space: O(n)
    
    Example: candidates = [10,1,2,7,6,1,5], target = 8
    Output: [[1,1,6],[1,2,5],[1,7],[2,6]]
    """
    result = []
    candidates.sort()
    
    def backtrack(start, path, remaining):
        if remaining == 0:
            result.append(path[:])
            return
        
        if remaining < 0:
            return
        
        for i in range(start, len(candidates)):
            # Skip duplicates
            if i > start and candidates[i] == candidates[i - 1]:
                continue
            
            path.append(candidates[i])
            backtrack(i + 1, path, remaining - candidates[i])
            path.pop()
    
    backtrack(0, [], target)
    return result

# Test
print(combination_sum2([10,1,2,7,6,1,5], 8))
```

### Problem 4: Letter Combinations of Phone Number

```python
def letter_combinations(digits: str) -> list[str]:
    """
    Generate all letter combinations from phone digits.
    
    Time: O(4^n) where n is digits length
    Space: O(n)
    
    Example: digits = "23"
    Output: ["ad","ae","af","bd","be","bf","cd","ce","cf"]
    """
    if not digits:
        return []
    
    phone = {
        '2': 'abc', '3': 'def', '4': 'ghi', '5': 'jkl',
        '6': 'mno', '7': 'pqrs', '8': 'tuv', '9': 'wxyz'
    }
    
    result = []
    
    def backtrack(index, path):
        if index == len(digits):
            result.append(''.join(path))
            return
        
        for letter in phone[digits[index]]:
            path.append(letter)
            backtrack(index + 1, path)
            path.pop()
    
    backtrack(0, [])
    return result

# Test
print(letter_combinations("23"))
```

---

## Permutations

### Problem 5: Permutations

```python
def permute(nums: list[int]) -> list[list[int]]:
    """
    Generate all permutations.
    
    Time: O(n!)
    Space: O(n)
    
    Example: nums = [1,2,3]
    Output: [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
    """
    result = []
    
    def backtrack(path, remaining):
        if not remaining:
            result.append(path[:])
            return
        
        for i in range(len(remaining)):
            path.append(remaining[i])
            backtrack(path, remaining[:i] + remaining[i+1:])
            path.pop()
    
    backtrack([], nums)
    return result

# Test
print(permute([1,2,3]))
```

### Problem 6: Permutations (Using Visited Array)

```python
def permute_visited(nums: list[int]) -> list[list[int]]:
    """
    Generate permutations using visited array.
    
    Time: O(n!)
    Space: O(n)
    """
    result = []
    visited = [False] * len(nums)
    
    def backtrack(path):
        if len(path) == len(nums):
            result.append(path[:])
            return
        
        for i in range(len(nums)):
            if visited[i]:
                continue
            
            visited[i] = True
            path.append(nums[i])
            backtrack(path)
            path.pop()
            visited[i] = False
    
    backtrack([])
    return result

# Test
print(permute_visited([1,2,3]))
```

### Problem 7: Permutations II (With Duplicates)

```python
def permute_unique(nums: list[int]) -> list[list[int]]:
    """
    Generate unique permutations (handle duplicates).
    
    Time: O(n!)
    Space: O(n)
    
    Example: nums = [1,1,2]
    Output: [[1,1,2],[1,2,1],[2,1,1]]
    """
    result = []
    nums.sort()
    visited = [False] * len(nums)
    
    def backtrack(path):
        if len(path) == len(nums):
            result.append(path[:])
            return
        
        for i in range(len(nums)):
            if visited[i]:
                continue
            
            # Skip duplicates: if current == previous and previous not used
            if i > 0 and nums[i] == nums[i-1] and not visited[i-1]:
                continue
            
            visited[i] = True
            path.append(nums[i])
            backtrack(path)
            path.pop()
            visited[i] = False
    
    backtrack([])
    return result

# Test
print(permute_unique([1,1,2]))
```

---

## Subsets

### Problem 8: Subsets

```python
def subsets(nums: list[int]) -> list[list[int]]:
    """
    Generate all subsets (power set).
    
    Time: O(2^n)
    Space: O(n)
    
    Example: nums = [1,2,3]
    Output: [[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]
    """
    result = []
    
    def backtrack(start, path):
        result.append(path[:])
        
        for i in range(start, len(nums)):
            path.append(nums[i])
            backtrack(i + 1, path)
            path.pop()
    
    backtrack(0, [])
    return result

# Test
print(subsets([1,2,3]))
```

### Problem 9: Subsets II (With Duplicates)

```python
def subsets_with_dup(nums: list[int]) -> list[list[int]]:
    """
    Generate unique subsets (handle duplicates).
    
    Time: O(2^n)
    Space: O(n)
    
    Example: nums = [1,2,2]
    Output: [[],[1],[1,2],[1,2,2],[2],[2,2]]
    """
    result = []
    nums.sort()
    
    def backtrack(start, path):
        result.append(path[:])
        
        for i in range(start, len(nums)):
            # Skip duplicates
            if i > start and nums[i] == nums[i - 1]:
                continue
            
            path.append(nums[i])
            backtrack(i + 1, path)
            path.pop()
    
    backtrack(0, [])
    return result

# Test
print(subsets_with_dup([1,2,2]))
```

### Problem 10: Partition to K Equal Sum Subsets

```python
def can_partition_k_subsets(nums: list[int], k: int) -> bool:
    """
    Check if array can be partitioned into k equal sum subsets.
    
    Time: O(k * 2^n)
    Space: O(n)
    
    Example: nums = [4,3,2,3,5,2,1], k = 4
    Output: True
    """
    total = sum(nums)
    if total % k:
        return False
    
    target = total // k
    nums.sort(reverse=True)
    visited = [False] * len(nums)
    
    def backtrack(index, count, current_sum):
        if count == k:
            return True
        
        if current_sum == target:
            return backtrack(0, count + 1, 0)
        
        for i in range(index, len(nums)):
            if visited[i] or current_sum + nums[i] > target:
                continue
            
            visited[i] = True
            if backtrack(i + 1, count, current_sum + nums[i]):
                return True
            visited[i] = False
        
        return False
    
    return backtrack(0, 0, 0)

# Test
print(can_partition_k_subsets([4,3,2,3,5,2,1], 4))  # True
```

---

## Constraint Satisfaction

### Problem 11: N-Queens

```python
def solve_n_queens(n: int) -> list[list[str]]:
    """
    Place n queens on n×n board.
    
    Time: O(n!)
    Space: O(n²)
    
    Example: n = 4
    Output: [[".Q..","...Q","Q...","..Q."],["..Q.","Q...","...Q",".Q.."]]
    """
    result = []
    board = [['.'] * n for _ in range(n)]
    
    def is_valid(row, col):
        # Check column
        for i in range(row):
            if board[i][col] == 'Q':
                return False
        
        # Check diagonal (top-left)
        i, j = row - 1, col - 1
        while i >= 0 and j >= 0:
            if board[i][j] == 'Q':
                return False
            i -= 1
            j -= 1
        
        # Check diagonal (top-right)
        i, j = row - 1, col + 1
        while i >= 0 and j < n:
            if board[i][j] == 'Q':
                return False
            i -= 1
            j += 1
        
        return True
    
    def backtrack(row):
        if row == n:
            result.append([''.join(row) for row in board])
            return
        
        for col in range(n):
            if is_valid(row, col):
                board[row][col] = 'Q'
                backtrack(row + 1)
                board[row][col] = '.'
    
    backtrack(0)
    return result

# Test
print(solve_n_queens(4))
```

### Problem 12: Sudoku Solver

```python
def solve_sudoku(board: list[list[str]]) -> None:
    """
    Solve Sudoku puzzle.
    
    Time: O(9^(n²))
    Space: O(1)
    
    Modifies board in-place.
    """
    def is_valid(row, col, num):
        # Check row
        for j in range(9):
            if board[row][j] == num:
                return False
        
        # Check column
        for i in range(9):
            if board[i][col] == num:
                return False
        
        # Check 3x3 box
        box_row, box_col = 3 * (row // 3), 3 * (col // 3)
        for i in range(box_row, box_row + 3):
            for j in range(box_col, box_col + 3):
                if board[i][j] == num:
                    return False
        
        return True
    
    def backtrack():
        for i in range(9):
            for j in range(9):
                if board[i][j] == '.':
                    for num in '123456789':
                        if is_valid(i, j, num):
                            board[i][j] = num
                            
                            if backtrack():
                                return True
                            
                            board[i][j] = '.'
                    
                    return False
        
        return True
    
    backtrack()

# Test
board = [
    ["5","3",".",".","7",".",".",".","."],
    ["6",".",".","1","9","5",".",".","."],
    [".","9","8",".",".",".",".","6","."],
    ["8",".",".",".","6",".",".",".","3"],
    ["4",".",".","8",".","3",".",".","1"],
    ["7",".",".",".","2",".",".",".","6"],
    [".","6",".",".",".",".","2","8","."],
    [".",".",".","4","1","9",".",".","5"],
    [".",".",".",".","8",".",".","7","9"]
]
solve_sudoku(board)
print(board)
```

### Problem 13: Word Search

```python
def exist(board: list[list[str]], word: str) -> bool:
    """
    Find if word exists in board.
    
    Time: O(m * n * 4^L) where L is word length
    Space: O(L)
    
    Example: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]]
             word = "ABCCED"
    Output: True
    """
    rows, cols = len(board), len(board[0])
    
    def backtrack(r, c, index):
        if index == len(word):
            return True
        
        if (r < 0 or r >= rows or c < 0 or c >= cols or
            board[r][c] != word[index]):
            return False
        
        # Mark visited
        temp = board[r][c]
        board[r][c] = '#'
        
        # Try all 4 directions
        found = (backtrack(r + 1, c, index + 1) or
                backtrack(r - 1, c, index + 1) or
                backtrack(r, c + 1, index + 1) or
                backtrack(r, c - 1, index + 1))
        
        # Restore cell
        board[r][c] = temp
        
        return found
    
    for i in range(rows):
        for j in range(cols):
            if backtrack(i, j, 0):
                return True
    
    return False

# Test
board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]]
print(exist(board, "ABCCED"))  # True
```

### Problem 14: Palindrome Partitioning

```python
def partition(s: str) -> list[list[str]]:
    """
    Partition string into palindromes.
    
    Time: O(n * 2^n)
    Space: O(n)
    
    Example: s = "aab"
    Output: [["a","a","b"],["aa","b"]]
    """
    def is_palindrome(substring):
        return substring == substring[::-1]
    
    result = []
    
    def backtrack(start, path):
        if start == len(s):
            result.append(path[:])
            return
        
        for end in range(start + 1, len(s) + 1):
            substring = s[start:end]
            if is_palindrome(substring):
                path.append(substring)
                backtrack(end, path)
                path.pop()
    
    backtrack(0, [])
    return result

# Test
print(partition("aab"))  # [["a","a","b"],["aa","b"]]
```

### Problem 15: Generate Parentheses

```python
def generate_parenthesis(n: int) -> list[str]:
    """
    Generate all valid parentheses combinations.
    
    Time: O(4^n / sqrt(n)) - Catalan number
    Space: O(n)
    
    Example: n = 3
    Output: ["((()))","(()())","(())()","()(())","()()()"]
    """
    result = []
    
    def backtrack(path, open_count, close_count):
        if len(path) == 2 * n:
            result.append(''.join(path))
            return
        
        # Add open parenthesis if possible
        if open_count < n:
            path.append('(')
            backtrack(path, open_count + 1, close_count)
            path.pop()
        
        # Add close parenthesis if valid
        if close_count < open_count:
            path.append(')')
            backtrack(path, open_count, close_count + 1)
            path.pop()
    
    backtrack([], 0, 0)
    return result

# Test
print(generate_parenthesis(3))
```

---

## Interview Tips

### 1. Backtracking Template

```python
def backtrack(path, choices):
    # Base case
    if is_solution(path):
        result.append(path.copy())
        return
    
    # Try each choice
    for choice in choices:
        # Make choice
        make_choice(path, choice)
        
        # Recurse
        backtrack(path, next_choices)
        
        # Undo choice (backtrack)
        undo_choice(path, choice)
```

### 2. Common Patterns

```python
# Combinations (order doesn't matter)
def backtrack(start, path):
    for i in range(start, n):
        path.append(i)
        backtrack(i + 1, path)  # i+1: no reuse
        path.pop()

# Permutations (order matters)
def backtrack(path, remaining):
    if not remaining:
        result.append(path[:])
    for i in range(len(remaining)):
        backtrack(path + [remaining[i]], 
                 remaining[:i] + remaining[i+1:])

# Subsets (include or exclude)
def backtrack(index, path):
    result.append(path[:])
    for i in range(index, len(nums)):
        path.append(nums[i])
        backtrack(i + 1, path)
        path.pop()
```

### 3. Optimization Techniques

```python
# 1. Sort input (for pruning)
nums.sort()

# 2. Skip duplicates
if i > start and nums[i] == nums[i-1]:
    continue

# 3. Early termination
if remaining < 0:
    return

# 4. Use visited array
visited = [False] * n

# 5. Modify in-place (save space)
board[r][c] = '#'  # Mark visited
# ... recurse ...
board[r][c] = original  # Restore
```

### 4. When to Use Backtracking

```python
# Use backtracking when:
✅ Need all solutions
✅ Can prune search space
✅ Constraint satisfaction problems
✅ Combinatorial problems

# Don't use when:
❌ Only need one solution (use greedy/DP)
❌ Can't prune effectively
❌ Better algorithm exists
```

---

## Practice Problems

### Easy
1. [Binary Watch](https://leetcode.com/problems/binary-watch/)
2. [Letter Case Permutation](https://leetcode.com/problems/letter-case-permutation/)

### Medium
1. [Combinations](https://leetcode.com/problems/combinations/)
2. [Combination Sum](https://leetcode.com/problems/combination-sum/)
3. [Combination Sum II](https://leetcode.com/problems/combination-sum-ii/)
4. [Combination Sum III](https://leetcode.com/problems/combination-sum-iii/)
5. [Letter Combinations of Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/)
6. [Permutations](https://leetcode.com/problems/permutations/)
7. [Permutations II](https://leetcode.com/problems/permutations-ii/)
8. [Subsets](https://leetcode.com/problems/subsets/)
9. [Subsets II](https://leetcode.com/problems/subsets-ii/)
10. [Word Search](https://leetcode.com/problems/word-search/)
11. [Palindrome Partitioning](https://leetcode.com/problems/palindrome-partitioning/)
12. [Generate Parentheses](https://leetcode.com/problems/generate-parentheses/)

### Hard
1. [N-Queens](https://leetcode.com/problems/n-queens/)
2. [N-Queens II](https://leetcode.com/problems/n-queens-ii/)
3. [Sudoku Solver](https://leetcode.com/problems/sudoku-solver/)
4. [Word Search II](https://leetcode.com/problems/word-search-ii/)
5. [Partition to K Equal Sum Subsets](https://leetcode.com/problems/partition-to-k-equal-sum-subsets/)

---

## Summary

### Key Takeaways
- ✅ **Try all possibilities** - Exhaustive search
- ✅ **Prune early** - Stop when constraints violated
- ✅ **Backtrack** - Undo choices to try alternatives
- ✅ **Three main types** - Combinations, permutations, subsets

### Quick Reference

```python
# Backtracking Template
def backtrack(path):
    if complete:
        result.append(path.copy())
        return
    
    for choice in choices:
        path.append(choice)
        backtrack(path)
        path.pop()

# Combinations
for i in range(start, n):
    backtrack(i + 1, path)

# Permutations
for i in range(len(remaining)):
    backtrack(remaining[:i] + remaining[i+1:])

# Subsets
result.append(path[:])
for i in range(start, n):
    backtrack(i + 1, path)
```

---

**Next**: [DFS & BFS →](../05-dfs-bfs/README.md)

**Happy Coding! 🚀**
