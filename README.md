# 🚀 Python Data Structures & Algorithms - Complete Interview Prep Guide

> **A comprehensive, beginner-friendly guide to mastering DSA in Python for coding interviews**

[![Last Updated](https://img.shields.io/badge/Last%20Updated-October%202025-brightgreen)]()
[![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)]()
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)]()

---

## 📖 About This Repository

This repository contains comprehensive notes, implementations, and practice problems for **Python Data Structures & Algorithms (DSA)** tailored for coding interview preparation. Whether you're just starting out or brushing up for FAANG interviews, this guide has you covered.

**Why This Guide Exists**: Technical coding interviews can feel overwhelming. You're expected to know dozens of data structures, recognize patterns in unfamiliar problems, write bug-free code under pressure, and analyze complexity on the spot. This guide was created to transform that overwhelming mountain of knowledge into a clear, step-by-step learning path.

**What You'll Find Here**: This isn't just a collection of code snippets. Each topic is explained from first principles with real-world analogies, visual explanations, and multiple examples. You'll understand not just *how* algorithms work, but *why* they work, *when* to use them, and *how* to recognize them in interview problems.

### 🎯 What Makes This Guide Special?

- **Beginner-Friendly**: Assumes no prior DSA knowledge. We start with "What is an array?" and build up to advanced graph algorithms. Every concept is explained as if you're learning it for the first time, with intuitive examples and clear reasoning.

- **Comprehensive Coverage**: 100+ topics, 500+ problems covering everything from basic arrays to advanced dynamic programming. This isn't a shortcut guide—it's a complete education in DSA that prepares you for any question an interviewer might ask.

- **Python-Specific**: Leverages Python's built-in features and idioms. You'll learn to use `collections.Counter`, `heapq`, `bisect`, and other Python tools that make solutions more elegant. Every code example uses modern Python 3.8+ syntax with type hints.

- **Interview-Focused**: Based on Blind 75, Grind 75, and real interview patterns from FAANG companies. Every problem is chosen because it teaches a pattern you'll see in actual interviews. You'll learn to recognize these patterns so you can solve new problems you've never seen before.

- **Theory + Practice**: Detailed explanations with working code. First you'll understand the theory (how and why it works), then see implementations (clean, commented code), then practice problems (ranging from easy to hard), and finally interview tips (common variations and edge cases).

- **Progressive Difficulty**: Easy → Medium → Hard progression within each topic. We don't throw you into hard problems immediately. You'll build confidence with easier problems first, then gradually increase difficulty as your pattern recognition improves.

---

## 📚 Table of Contents

### [Part 1: Python Fundamentals](./01-python-fundamentals/README.md)
**Essential Python concepts needed for DSA**

Before diving into data structures and algorithms, you need to be comfortable with Python itself. This section covers everything from basic syntax to advanced features like decorators and generators. You'll learn the Python-specific tools that make coding interviews easier: list comprehensions for concise code, the `collections` module for powerful data structures, and built-in functions that replace common algorithms.

**What you'll learn**: Python basics (variables, control flow, functions), data types and operations, object-oriented programming, lambda functions, comprehensions, the `collections` module (`Counter`, `defaultdict`, `deque`), `itertools` and `functools`, and how to write clean, Pythonic code.

**Topics covered**:
- Python Basics & Syntax
- Data Types & Operations
- Functions & Lambda
- List Comprehensions & Generators
- OOP Concepts
- Python Built-in Functions
- Collections Module
- Itertools & Functools

### [Part 2: Complexity Analysis](./02-complexity-analysis/README.md)
**Understanding algorithm efficiency**

Knowing *if* your code works isn't enough—you need to know *how fast* it works and *how much memory* it uses. Complexity analysis is the language of algorithm efficiency. In interviews, you're expected to analyze the time and space complexity of your solutions and explain why one approach is better than another.

**What you'll learn**: How to use Big O notation to describe algorithm performance, how to calculate time and space complexity, common complexity patterns (O(1), O(n), O(log n), O(n²)), when to optimize and when "good enough" is acceptable, and how to recognize complexity in nested loops, recursion, and data structure operations.

**Topics covered**:
- Time Complexity (Big O, Omega, Theta)
- Space Complexity
- Amortized Analysis
- Common Complexity Patterns
- How to Calculate Complexity
- Optimization Techniques

### [Part 3: Data Structures](./03-data-structures/README.md)
**Core data structures with complete implementations**

Data structures are the foundation of all algorithms. They're the tools in your toolbox—you need to know what each one does, when to use it, and how it works under the hood. This section goes deep into 11 essential data structures with full implementations, visual explanations, and practice problems.

#### Linear Data Structures
**Sequential collections where elements are arranged in order**

- [Arrays & Lists](./03-data-structures/01-arrays-lists/README.md) - The most basic data structure. Learn dynamic arrays, common operations (insert, delete, search), two-pointer techniques, and how Python lists work internally.

- [Strings](./03-data-structures/02-strings/README.md) - Specialized arrays of characters. Master string manipulation, pattern matching, palindrome checking, anagram detection, and string-specific algorithms.

- [Linked Lists](./03-data-structures/03-linked-lists/README.md) - Nodes connected by pointers. Understand singly/doubly/circular linked lists, when they beat arrays, fast & slow pointer techniques, and how to reverse/merge/detect cycles.

- [Stacks](./03-data-structures/04-stacks/README.md) - Last-In-First-Out (LIFO) structures. Learn stack operations, when to use stacks (parentheses matching, DFS, undo operations), monotonic stacks, and stack-based algorithms.

- [Queues](./03-data-structures/05-queues/README.md) - First-In-First-Out (FIFO) structures. Master queue operations, circular queues, deques, priority queues, and when queues are the right choice (BFS, scheduling).

#### Non-Linear Data Structures
**Hierarchical and graph-based collections**

- [Hash Tables](./03-data-structures/06-hash-tables/README.md) - O(1) average lookup time. Understand hash functions, collision resolution (chaining vs. open addressing), when to use dictionaries vs. sets, and how Python's `dict` works.

- [Trees](./03-data-structures/07-trees/README.md) - Hierarchical structures with roots and children. Master binary trees, tree traversals (inorder, preorder, postorder, level-order), tree construction, and common tree problems.
  - Binary Trees - Basic tree structure
  - Binary Search Trees - Sorted trees with O(log n) operations
  - AVL Trees - Self-balancing BSTs
  - Heaps - Complete binary trees for priority operations
  - Tries - Prefix trees for string operations

- [Graphs](./03-data-structures/08-graphs/README.md) - Nodes connected by edges. Learn graph representations (adjacency matrix vs. list), traversals (DFS, BFS), shortest path algorithms (Dijkstra, Bellman-Ford), and advanced topics (topological sort, MST).

#### Advanced Data Structures
**Specialized structures for specific problems**

- [Disjoint Set Union (Union-Find)](./03-data-structures/09-union-find/README.md) - Track connected components efficiently with near-constant time operations using path compression and union by rank.

- [Segment Trees](./03-data-structures/10-segment-trees/README.md) - Range queries and updates in O(log n) time, perfect for problems involving intervals.

- [Fenwick Trees](./03-data-structures/11-fenwick-trees/README.md) - Binary Indexed Trees for efficient prefix sum queries and point updates.

### [Part 4: Algorithm Patterns](./04-algorithm-patterns/README.md)
**Essential problem-solving patterns**

Most interview problems aren't completely unique—they follow recognizable patterns. Once you learn these patterns, you can solve hundreds of problems by recognizing which pattern applies. This section teaches you 15 core patterns with templates, examples, and practice problems.

**Why patterns matter**: Instead of memorizing solutions to 500 individual problems, you learn 15 patterns that unlock those 500 problems. Pattern recognition is the skill that separates candidates who struggle with new problems from those who solve them confidently.

- [Two Pointers](./04-algorithm-patterns/01-two-pointers/README.md) - Use two pointers moving through data to reduce O(n²) to O(n). Perfect for sorted arrays, palindrome checking, and finding pairs.

- [Sliding Window](./04-algorithm-patterns/02-sliding-window/README.md) - Maintain a window that slides through data to track substrings or subarrays efficiently. Essential for substring problems and contiguous sequences.

- [Fast & Slow Pointers](./04-algorithm-patterns/03-fast-slow-pointers/README.md) - Floyd's cycle detection and finding middle elements. The key to linked list cycle problems and certain array problems.

- [Binary Search](./04-algorithm-patterns/04-binary-search/README.md) - Reduce O(n) search to O(log n) by eliminating half the search space each step. Works on sorted data and "monotonic" functions.

- [Sorting Algorithms](./04-algorithm-patterns/05-sorting/README.md) - All major sorting algorithms from bubble sort to quicksort. Understand when to use each and how they work internally.

- [Recursion & Backtracking](./04-algorithm-patterns/06-recursion-backtracking/README.md) - Break problems into smaller subproblems and explore all possibilities. Essential for combinations, permutations, and constraint satisfaction problems.

- [Dynamic Programming](./04-algorithm-patterns/07-dynamic-programming/README.md) - Optimize recursive solutions by storing intermediate results. The most powerful pattern for optimization problems.

- [Greedy Algorithms](./04-algorithm-patterns/08-greedy/README.md) - Make the locally optimal choice at each step. Works when local optimality leads to global optimality (intervals, scheduling).

- [Graph Algorithms](./04-algorithm-patterns/09-graph-algorithms/README.md) - DFS, BFS, shortest paths, topological sort, minimum spanning trees. Everything you need for graph interview problems.
  - DFS & BFS
  - Dijkstra's Algorithm
  - Bellman-Ford
  - Floyd-Warshall
  - Topological Sort
  - Minimum Spanning Tree

- [Bit Manipulation](./04-algorithm-patterns/10-bit-manipulation/README.md) - Solve problems using bitwise operations. Powerful tricks for set operations, number manipulation, and optimization.

- [Math & Geometry](./04-algorithm-patterns/11-math-geometry/README.md) - Mathematical algorithms (GCD, primes, modular arithmetic) and geometric problems (rotation, coordinates).

- [Intervals](./04-algorithm-patterns/12-intervals/README.md) - Merge overlapping intervals, schedule meetings, and handle range problems. Common in scheduling and calendar problems.

- [Matrix Patterns](./04-algorithm-patterns/13-matrix/README.md) - Traverse 2D arrays efficiently, rotate matrices, search in sorted matrices. Essential for grid-based problems.

- [Divide & Conquer](./04-algorithm-patterns/14-divide-conquer/README.md) - Split problems in half, solve recursively, then combine. The principle behind merge sort, quicksort, and binary search.

- [Monotonic Stack/Queue](./04-algorithm-patterns/15-monotonic/README.md) - Maintain monotonic order to find next greater/smaller elements in O(n). A hidden pattern in many array problems.

### [Part 5: Problem-Solving Strategies](./05-problem-solving/README.md)
**How to think through problems systematically**

Having knowledge isn't enough—you need a systematic approach to apply it. This section teaches you how to tackle unfamiliar problems, recognize patterns, optimize solutions, and communicate your thinking clearly.

**What you'll learn**: Step-by-step problem-solving framework, how to recognize which pattern or data structure to use, common tricks and optimizations, how to debug efficiently, how to design comprehensive test cases, and how to explain your solution clearly in interviews.
**What you'll learn**: Step-by-step problem-solving framework, how to recognize which pattern or data structure to use, common tricks and optimizations, how to debug efficiently, how to design comprehensive test cases, and how to explain your solution clearly in interviews.

**Topics covered**:
- How to Approach a Problem
- Pattern Recognition
- Common Tricks & Optimizations
- Debugging Strategies
- Test Case Design
- Interview Communication

### [Part 6: Famous Interview Questions](./06-interview-questions/README.md)
**Curated lists of must-solve problems**

These aren't random problems—they're carefully selected questions that appear frequently in real interviews at top companies. Each list is battle-tested and represents the most common patterns and concepts interviewers test for.

**Why these specific lists**: The Blind 75 was created by a Meta engineer who analyzed thousands of LeetCode questions to find the 75 that give you the most pattern coverage. Grind 75 is its successor with more modern problems. NeetCode 150 expands coverage to advanced topics. Together, these lists ensure you're prepared for 95%+ of interview questions.

- [Blind 75 Problems](./06-interview-questions/blind-75/README.md) - The gold standard. 75 problems that cover all essential patterns. If you can solve these, you can solve most interview questions.

- [Grind 75 Problems](./06-interview-questions/grind-75/README.md) - Updated version of Blind 75 with difficulty progression. Organized by week for structured practice.

- [NeetCode 150](./06-interview-questions/neetcode-150/README.md) - Comprehensive list covering advanced patterns. For candidates targeting senior roles or wanting deep mastery.

- [Top 100 Liked LeetCode](./06-interview-questions/top-100/README.md) - Community favorites that are both educational and frequently asked.

- [Company-Specific Problems](./06-interview-questions/company-specific/README.md) - Questions actually asked at Google, Meta, Amazon, Microsoft, Apple, and other top companies.

### [Part 7: Study Plans](./07-study-plans/README.md)
**Structured learning paths for different timelines**

Having 500 problems is overwhelming. These study plans give you structure, telling you exactly what to study each day based on how much time you have before interviews.

**Choose your timeline**: Got 4 weeks? Follow the crash course focusing on high-frequency patterns. Have 12 weeks? Do the deep dive covering everything comprehensively. Each plan builds progressively, ensuring you master fundamentals before advancing.

- [4-Week Crash Course](./07-study-plans/4-week-crash.md) - For urgent interview prep. Focus on Blind 75 and core patterns. ~15 hours/week.

- [8-Week Comprehensive Plan](./07-study-plans/8-week-comprehensive.md) - Balanced approach covering all patterns and 100+ problems. ~12 hours/week.

- [12-Week Deep Dive](./07-study-plans/12-week-deep.md) - Complete mastery from basics to advanced topics. ~10 hours/week.

- [Topic-Wise Practice Schedule](./07-study-plans/topic-wise.md) - Study by topic (all array problems, then all tree problems, etc.) rather than chronologically.

### [Part 8: Resources & References](./08-resources/README.md)
**Recommended learning materials and tools**

Beyond this guide, these resources will deepen your understanding and give you additional practice. Each resource is chosen for quality and relevance to technical interviews.

**Topics covered**:
- Books (Cracking the Coding Interview, Elements of Programming Interviews)
- Online Courses (best video courses for visual learners)
- Practice Platforms (LeetCode, HackerRank, CodeSignal)
- YouTube Channels (NeetCode, Back To Back SWE, William Fiset)
- Cheat Sheets (quick references for each topic)
- Interview Tips (what to expect, how to communicate, common mistakes)

---

## 🎓 How to Use This Guide

### For Complete Beginners
**"I have no DSA background and want to start from scratch"**

If you're new to data structures and algorithms, welcome! This guide is designed with you in mind. Don't feel overwhelmed by the amount of content—we'll build your skills progressively.

**Your learning path**:
1. **Start with [Python Fundamentals](./01-python-fundamentals/README.md)** (1 week)
   - Even if you know Python, review this section. You'll learn Python-specific tricks that make DSA problems easier (list comprehensions, `collections` module, etc.)
   - Make sure you understand basic data types, control flow, and functions before moving on.

2. **Learn [Complexity Analysis](./02-complexity-analysis/README.md)** (2-3 days)
   - This is your new language for talking about algorithm efficiency. Every interview will expect you to analyze time and space complexity.
   - Practice identifying O(n), O(log n), O(n²) by looking at code with loops and recursion.

3. **Follow the [12-Week Deep Dive Plan](./07-study-plans/12-week-deep.md)** (3 months)
   - This plan is specifically designed for beginners, starting with easier concepts and building up.
   - Spend 10-15 hours per week: study theory, implement data structures, solve practice problems.
   - Don't skip ahead! Each week builds on previous weeks.

4. **Practice problems in order: Easy → Medium → Hard**
   - Start with easy problems to build confidence and pattern recognition.
   - Don't get frustrated if medium problems take time—that's normal!
   - Only attempt hard problems after mastering medium ones.

**Beginner tips**:
- It's okay to look at solutions! But always reimplement from scratch afterward.
- Focus on understanding *why* a solution works, not just *what* the code does.
- Use a notebook to write down patterns and insights—your brain will thank you.
- Practice consistency over intensity: 1 hour daily beats 7 hours on Sunday.

### For Interview Prep (4-8 Weeks)
**"I have interviews coming up and need to prepare quickly"**

You don't have time for everything, so we'll focus on high-frequency patterns and essential problems.

**Your learning path**:
1. **Review Python fundamentals (if needed)** (1-2 days)
   - Make sure you're comfortable with dictionaries, sets, list comprehensions, and the `collections` module.
   - These tools will make your interview code cleaner and faster to write.

2. **Focus on [Algorithm Patterns](./04-algorithm-patterns/README.md)** (2-3 weeks)
   - These patterns unlock most interview problems. Prioritize:
     - Two Pointers & Sliding Window (arrays/strings)
     - Fast & Slow Pointers (linked lists)
     - Binary Search (sorted arrays)
     - DFS & BFS (trees/graphs)
     - Dynamic Programming (optimization problems)
   - For each pattern: understand the template, solve 3-5 problems, move to the next.

3. **Solve [Blind 75](./06-interview-questions/blind-75/README.md) or [Grind 75](./06-interview-questions/grind-75/README.md)** (2-4 weeks)
   - Blind 75 is more focused (75 problems), Grind 75 is better organized by difficulty.
   - Try each problem yourself for 20-30 minutes before looking at the solution.
   - For each problem, understand multiple approaches and their tradeoffs.

4. **Practice [Company-Specific Problems](./06-interview-questions/company-specific/README.md)** (1 week)
   - If you know which company you're interviewing with, focus on their common question types.
   - Google loves graph problems, Amazon loves trees, Meta loves medium-difficulty mixed problems.

**Interview prep tips**:
- Do timed practice to simulate real interview pressure (45 minutes per problem).
- Practice explaining your thought process out loud—communication is half the battle.
- Review your mistakes: keep a list of problems you got wrong and why.
- Don't neglect easy problems—they test if you can code quickly without bugs.

### For Quick Revision
**"I already know DSA and just need to refresh before an interview"**

You need targeted review of concepts and patterns, not learning from scratch.

**Your learning path**:
1. **Use topic-specific cheat sheets** (2-3 days)
   - Each data structure and pattern chapter has a summary section.
   - Review the key templates, time complexities, and common pitfalls.
   - Make flashcards for O(n) operations on different data structures.

2. **Review pattern summaries** (1 week)
   - For each of the 15 patterns, review the "When to Use" and "Template" sections.
   - Make sure you can recognize each pattern in a problem description.
   - Understand the time/space complexity of each pattern's template.

3. **Solve one problem from each pattern** (1 week)
   - Pick a medium-difficulty problem from each pattern to verify you remember it.
   - If you struggle with a pattern, solve 2-3 more problems from it.
   - Focus on writing clean, bug-free code quickly.

4. **Focus on weak areas** (1 week)
   - Be honest about which topics you're rusty on (usually DP, graphs, or backtracking).
   - Spend extra time on these topics: theory review + practice problems.
   - Do mock interviews to identify unexpected weak spots.

**Quick revision tips**:
- Quality over quantity—solving 5 problems deeply is better than 20 problems superficially.
- Practice on paper or a whiteboard to simulate real interview conditions.
- Time yourself: can you solve an easy in 15 min, medium in 30 min, hard in 45 min?
- Review common bugs: off-by-one errors, forgetting edge cases, integer overflow.

---

## 📊 Progress Tracking

**Why tracking matters**: Studies show that tracking progress increases motivation and completion rates. Seeing your checkboxes fill up creates momentum and helps you identify which areas need more attention.

Create a copy of this checklist to track your progress. Update it weekly and celebrate small wins!

### Data Structures Progress
Check off each data structure after you:
- ✅ Understand the theory (how it works, when to use it)
- ✅ Can implement it from scratch
- ✅ Solved at least 5 practice problems using it
- ✅ Know its time/space complexity for all operations

Checklist:
- [ ] Arrays & Lists - The foundation. Master indexing, slicing, and common operations.
- [ ] Strings - Character arrays with special methods. Essential for string manipulation problems.
- [ ] Linked Lists - Pointer-based structures. Key for many technical interviews.
- [ ] Stacks - LIFO structure. Used in DFS, expression evaluation, and monotonic problems.
- [ ] Queues - FIFO structure. Used in BFS, scheduling, and streaming data.
- [ ] Hash Tables - O(1) lookup. The most common optimization technique.
- [ ] Trees - Hierarchical data. Foundation for many advanced algorithms.
- [ ] Graphs - Most versatile structure. Models networks, dependencies, and relationships.
- [ ] Heaps - Priority operations. Essential for scheduling and "top K" problems.
- [ ] Tries - Prefix trees. Key for autocomplete and word search problems.

### Algorithm Patterns Progress
Check off each pattern after you:
- ✅ Understand when to apply it
- ✅ Know the template/approach by heart
- ✅ Solved at least 5-7 problems using it
- ✅ Can recognize it in new problems

Checklist:
- [ ] Two Pointers - Reduces O(n²) to O(n). Used in 50+ Blind 75 variations.
- [ ] Sliding Window - Subarray/substring problems. Essential for string problems.
- [ ] Binary Search - O(log n) search. More than just searching sorted arrays!
- [ ] Sorting - Foundation for many optimizations. Know when and which algorithm to use.
- [ ] Recursion & Backtracking - Generate all possibilities. Key for combinations/permutations.
- [ ] Dynamic Programming - The hardest pattern but appears in 20%+ of hard problems.
- [ ] Greedy - Make locally optimal choices. Simpler than DP when it works.
- [ ] Graph Algorithms - DFS, BFS, Dijkstra, topological sort. Must-know for graph problems.
- [ ] Bit Manipulation - Clever tricks for set operations and optimization.
- [ ] Math & Geometry - Number theory and geometric algorithms.

### Problem Sets Progress
Track your completion of famous problem lists. Aim for understanding, not just completion!

- [ ] Blind 75 (___/75) - 🎯 Target: 100%. This is the minimum for interview readiness.
- [ ] Grind 75 (___/75) - 🎯 Target: 80-100%. Modern alternative to Blind 75.
- [ ] NeetCode 150 (___/150) - 🎯 Target: 50-100%. For comprehensive preparation.

**Tracking tips**:
- Don't just mark problems as "done"—can you solve them again without hints?
- Revisit problems you struggled with after 1 week, then 1 month (spaced repetition).
- Keep a journal noting what you learned from each problem.
- Track your time: are you getting faster? That shows growing pattern recognition.

---

## 🛠️ Python Setup

**Setting up your environment correctly saves hours of debugging later**

### Required Python Version
```bash
# Check your Python version (must be 3.8 or higher)
python --version  # Should show Python 3.8+

# If you have both Python 2 and 3, you might need:
python3 --version
```

**Why Python 3.8+?** This version introduced several features we use throughout this guide:
- Type hints with `list[int]` instead of `List[int]`
- Assignment expressions (walrus operator `:=`)
- Positional-only parameters
- Improved performance for dictionary operations

### Install Helpful Libraries

These libraries aren't required for solving problems, but they're incredibly useful for testing and analyzing your code:

```bash
# For writing and running test cases
pip install pytest

# For analyzing memory usage of your solutions
pip install memory-profiler

# For identifying performance bottlenecks
pip install line-profiler

# For visualizing data structures (helpful for learning)
pip install graphviz
```

### IDE Setup Recommendations

**VS Code** (recommended):
- Install Python extension
- Enable type checking: add `"python.analysis.typeCheckingMode": "basic"` to settings
- Use pylint for style checking
- Set up auto-formatting with black or autopep8

**PyCharm**:
- Excellent autocomplete and debugging
- Built-in type checking
- Good for larger projects

**LeetCode Editor Plugin**:
- Practice directly in your IDE
- Test with custom test cases
- Track your progress locally

### Verify Your Setup

Run this test script to make sure everything works:

```python
# test_setup.py
from collections import Counter, defaultdict, deque
from typing import List, Dict, Set
import heapq
import bisect

def test_basic_functionality():
    """Test that all essential libraries and features work"""
    # Test type hints
    def add_numbers(a: int, b: int) -> int:
        return a + b
    
    # Test collections
    counter = Counter([1, 2, 2, 3, 3, 3])
    assert counter[3] == 3
    
    # Test heap
    heap = [3, 1, 4, 1, 5]
    heapq.heapify(heap)
    assert heapq.heappop(heap) == 1
    
    # Test bisect
    arr = [1, 3, 5, 7, 9]
    assert bisect.bisect_left(arr, 5) == 2
    
    print("✅ All tests passed! Your setup is ready.")

if __name__ == "__main__":
    test_basic_functionality()
```

If this runs without errors, you're ready to start!

---

## 📝 Code Style & Conventions

**Why coding style matters in interviews**: Clean, readable code shows professionalism and makes it easier for interviewers to understand your logic. Following conventions also reduces bugs and makes your code easier to debug under pressure.

All code in this repository follows these principles:

### 1. PEP 8 Style Guidelines
**The official Python style guide**—following it makes your code look professional and Pythonic.

Key rules we follow:
- **4 spaces for indentation** (not tabs)
- **snake_case for functions and variables**: `calculate_sum`, not `calculateSum`
- **PascalCase for classes**: `TreeNode`, `LinkedList`
- **UPPER_CASE for constants**: `MAX_SIZE = 100`
- **2 blank lines between functions**
- **Meaningful variable names**: `head`, `current`, `target` instead of `h`, `c`, `t`

### 2. Type Hints for Clarity
**Modern Python uses type hints** to make function signatures clear and catch bugs early.

We use type hints on:
- Function parameters
- Return values
- Complex data structures

```python
# Good: Clear what types are expected and returned
def find_pair(nums: list[int], target: int) -> list[int]:
    pass

# Bad: No hints about what types to use
def find_pair(nums, target):
    pass
```

### 3. Comprehensive Docstrings
**Every function explains what it does, its complexity, parameters, and return value.**

Our docstring format:
```python
def binary_search(arr: list[int], target: int) -> int:
    """
    Find the index of target in sorted array using binary search.
    
    This algorithm repeatedly divides the search interval in half,
    eliminating half of the remaining elements each time. This gives
    us logarithmic time complexity.
    
    Time Complexity: O(log n) - We halve the search space each iteration
    Space Complexity: O(1) - Only using a constant number of pointers
    
    Args:
        arr: Sorted list of integers to search in
        target: Integer value we're looking for
        
    Returns:
        Index of target in arr, or -1 if not found
        
    Example:
        >>> binary_search([1, 3, 5, 7, 9], 5)
        2
        >>> binary_search([1, 3, 5, 7, 9], 4)
        -1
    """
    left, right = 0, len(arr) - 1
    
    while left <= right:
        mid = left + (right - left) // 2  # Avoid overflow
        
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            left = mid + 1  # Target is in right half
        else:
            right = mid - 1  # Target is in left half
    
    return -1  # Target not found
```

### 4. Unit Tests for Validation
**We test edge cases to ensure solutions are correct.**

Test cases include:
- Empty input
- Single element
- Two elements
- Typical case
- Large input
- Edge values (negative numbers, duplicates, etc.)

```python
def test_binary_search():
    """Comprehensive tests for binary_search function"""
    # Empty array
    assert binary_search([], 5) == -1
    
    # Single element - found
    assert binary_search([5], 5) == 0
    
    # Single element - not found
    assert binary_search([5], 3) == -1
    
    # Typical case - found at different positions
    assert binary_search([1, 3, 5, 7, 9], 1) == 0  # First
    assert binary_search([1, 3, 5, 7, 9], 5) == 2  # Middle
    assert binary_search([1, 3, 5, 7, 9], 9) == 4  # Last
    
    # Typical case - not found
    assert binary_search([1, 3, 5, 7, 9], 4) == -1
    
    # Large array
    assert binary_search(list(range(1000)), 500) == 500
```

### 5. Complexity Analysis in Comments
**Every solution explains its time and space complexity with reasoning.**

We explain:
- What causes the time complexity (loops, recursion depth)
- What causes the space complexity (data structures, recursion stack)
- Why this is the optimal complexity (or why not)

```python
def find_duplicates(nums: list[int]) -> list[int]:
    """
    Find all numbers that appear more than once.
    
    Approach: Use a hash set to track seen numbers.
    
    Time Complexity: O(n)
    - We iterate through n elements once
    - Hash set operations (add, lookup) are O(1) average
    - Total: O(n) * O(1) = O(n)
    
    Space Complexity: O(n)
    - Hash set can store up to n elements
    - Result list can store up to n elements
    - Total: O(n) + O(n) = O(n)
    
    Alternative O(1) space approach exists but modifies input array.
    """
    seen = set()
    duplicates = []
    
    for num in nums:
        if num in seen:
            duplicates.append(num)
        seen.add(num)
    
    return duplicates
```

### Interview-Ready Code Template

Here's the template we use for interview solutions:

```python
def two_sum(nums: list[int], target: int) -> list[int]:
    """
    Find two numbers that add up to target.
    
    Approach: Use hash map to store complements.
    As we iterate, check if the current number's complement
    (target - current) exists in our hash map. If yes, we found
    our pair. If no, store current number for future lookups.
    
    Time Complexity: O(n)
    - Single pass through array: O(n)
    - Hash map operations: O(1) average
    
    Space Complexity: O(n)
    - Hash map stores up to n elements
    
    Args:
        nums: List of integers
        target: Target sum to find
        
    Returns:
        List containing two indices [i, j] where nums[i] + nums[j] = target
        
    Example:
        >>> two_sum([2, 7, 11, 15], 9)
        [0, 1]
    """
    # Hash map to store: number -> index
    seen = {}
    
    # Iterate through array
    for i, num in enumerate(nums):
        # Calculate what number we need to reach target
        complement = target - num
        
        # If complement exists in map, we found our answer
        if complement in seen:
            return [seen[complement], i]
        
        # Store current number and its index for future lookups
        seen[num] = i
    
    # No solution found (problem guarantees solution exists)
    return []
```

**Key principles**:
- Clear, descriptive variable names
- Comments explain *why*, not *what* (code shows what)
- Type hints on all parameters and returns
- Docstring with complexity analysis
- Example usage
- Clean, readable logic

---

## 🤝 Contributing

Found an error? Want to add more problems or explanations? Contributions are welcome!

---

## 📜 License

This repository is for educational purposes. Feel free to use and share.

---

## ⭐ Acknowledgments

This guide is inspired by:
- **Blind 75** by Yangshun Tay
- **Grind 75** (successor to Blind 75)
- **NeetCode** roadmap and explanations
- **GeeksforGeeks** DSA roadmap
- **LeetCode** problem patterns
- **Tech Interview Handbook**

---

## 📫 Contact & Feedback

Questions or suggestions? Feel free to open an issue or reach out!

---

### 🎯 Quick Links

- [Start Learning](./01-python-fundamentals/README.md)
- [Jump to Patterns](./04-algorithm-patterns/README.md)
- [Browse Problems](./06-interview-questions/README.md)
- [Study Plans](./07-study-plans/README.md)

---

**Last Updated**: October 21, 2025

**Happy Coding! 🚀**
