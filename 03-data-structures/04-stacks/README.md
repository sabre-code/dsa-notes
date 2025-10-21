# 📚 Stacks - Python DSA

> Last In, First Out (LIFO) data structure

---

## 📚 Table of Contents

1. [Introduction](#introduction)
2. [Implementation](#implementation)
3. [Common Patterns](#common-patterns)
4. [Classic Problems](#classic-problems)
5. [Interview Tips](#interview-tips)
6. [Practice Problems](#practice-problems)

---

## Introduction

**Stack** is a linear data structure that follows LIFO (Last In, First Out) principle.

### Key Characteristics
- ✅ **LIFO access** - Last element added is first removed
- ✅ **O(1) push/pop** - Constant time operations
- ✅ **Simple implementation** - Using list or linked list
- ❌ **No random access** - Can only access top element

### Real-World Examples
- 📚 Stack of plates
- ↩️ Browser back button
- ⌨️ Undo/Redo functionality
- 🔄 Function call stack
- 🧮 Expression evaluation

### Stack Operations

| Operation | Time | Description |
|-----------|------|-------------|
| push(item) | O(1) | Add to top |
| pop() | O(1) | Remove from top |
| peek() | O(1) | View top element |
| is_empty() | O(1) | Check if empty |
| size() | O(1) | Get size |

---

## Implementation

### Using Python List

```python
class Stack:
    """Stack using Python list."""
    
    def __init__(self):
        self.items = []
    
    def push(self, item):
        """Add item to top. Time: O(1)"""
        self.items.append(item)
    
    def pop(self):
        """Remove and return top item. Time: O(1)"""
        if self.is_empty():
            raise IndexError("Stack is empty")
        return self.items.pop()
    
    def peek(self):
        """Return top item without removing. Time: O(1)"""
        if self.is_empty():
            raise IndexError("Stack is empty")
        return self.items[-1]
    
    def is_empty(self):
        """Check if stack is empty. Time: O(1)"""
        return len(self.items) == 0
    
    def size(self):
        """Return size of stack. Time: O(1)"""
        return len(self.items)
    
    def __repr__(self):
        return f"Stack({self.items})"

# Test
stack = Stack()
stack.push(1)
stack.push(2)
stack.push(3)
print(stack.pop())  # 3
print(stack.peek())  # 2
```

### Using Linked List

```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

class LinkedStack:
    """Stack using linked list."""
    
    def __init__(self):
        self.head = None
        self._size = 0
    
    def push(self, item):
        """Add item to top. Time: O(1)"""
        new_node = Node(item)
        new_node.next = self.head
        self.head = new_node
        self._size += 1
    
    def pop(self):
        """Remove and return top item. Time: O(1)"""
        if self.is_empty():
            raise IndexError("Stack is empty")
        
        data = self.head.data
        self.head = self.head.next
        self._size -= 1
        return data
    
    def peek(self):
        """Return top item without removing. Time: O(1)"""
        if self.is_empty():
            raise IndexError("Stack is empty")
        return self.head.data
    
    def is_empty(self):
        """Check if stack is empty. Time: O(1)"""
        return self.head is None
    
    def size(self):
        """Return size of stack. Time: O(1)"""
        return self._size
```

---

## Common Patterns

### Pattern 1: Monotonic Stack

Stack maintains elements in monotonically increasing or decreasing order.

```python
def next_greater_elements(nums: list[int]) -> list[int]:
    """
    Find next greater element for each element.
    
    Time: O(n)
    Space: O(n)
    """
    n = len(nums)
    result = [-1] * n
    stack = []  # Store indices
    
    for i in range(n):
        # Pop elements smaller than current
        while stack and nums[stack[-1]] < nums[i]:
            index = stack.pop()
            result[index] = nums[i]
        stack.append(i)
    
    return result

# Example:
# Input: [2, 1, 2, 4, 3]
# Output: [4, 2, 4, -1, -1]
```

### Pattern 2: Expression Evaluation

```python
def evaluate_postfix(expression: str) -> int:
    """
    Evaluate postfix expression.
    
    Time: O(n)
    Space: O(n)
    """
    stack = []
    operators = {'+', '-', '*', '/'}
    
    for char in expression.split():
        if char not in operators:
            stack.append(int(char))
        else:
            b = stack.pop()
            a = stack.pop()
            
            if char == '+':
                stack.append(a + b)
            elif char == '-':
                stack.append(a - b)
            elif char == '*':
                stack.append(a * b)
            elif char == '/':
                stack.append(int(a / b))
    
    return stack[-1]

# Example:
# Input: "2 1 + 3 *"
# Output: 9  # (2 + 1) * 3
```

### Pattern 3: Bracket Matching

```python
def is_valid_brackets(s: str) -> bool:
    """
    Check if brackets are balanced.
    
    Time: O(n)
    Space: O(n)
    """
    stack = []
    mapping = {')': '(', '}': '{', ']': '['}
    
    for char in s:
        if char in mapping:
            top = stack.pop() if stack else '#'
            if mapping[char] != top:
                return False
        else:
            stack.append(char)
    
    return not stack
```

### Pattern 4: Backtracking with Stack

```python
def backtrack_with_stack():
    """Template for backtracking using stack."""
    stack = [(initial_state, [])]  # (state, path)
    results = []
    
    while stack:
        state, path = stack.pop()
        
        if is_goal(state):
            results.append(path[:])
            continue
        
        for next_state in get_next_states(state):
            if is_valid(next_state):
                stack.append((next_state, path + [next_state]))
    
    return results
```

---

## Classic Problems

### 1. Valid Parentheses

```python
def is_valid(s: str) -> bool:
    """
    Check if string has valid parentheses.
    
    Time: O(n)
    Space: O(n)
    """
    stack = []
    mapping = {')': '(', '}': '{', ']': '['}
    
    for char in s:
        if char in mapping:
            if not stack or stack[-1] != mapping[char]:
                return False
            stack.pop()
        else:
            stack.append(char)
    
    return len(stack) == 0

# Examples:
# "()" -> True
# "()[]{}" -> True
# "(]" -> False
# "([)]" -> False
# "{[]}" -> True
```

### 2. Min Stack

```python
class MinStack:
    """
    Stack with O(1) min operation.
    """
    
    def __init__(self):
        self.stack = []
        self.min_stack = []
    
    def push(self, val: int) -> None:
        """Time: O(1)"""
        self.stack.append(val)
        
        # Update min stack
        if not self.min_stack or val <= self.min_stack[-1]:
            self.min_stack.append(val)
    
    def pop(self) -> None:
        """Time: O(1)"""
        if self.stack[-1] == self.min_stack[-1]:
            self.min_stack.pop()
        self.stack.pop()
    
    def top(self) -> int:
        """Time: O(1)"""
        return self.stack[-1]
    
    def get_min(self) -> int:
        """Time: O(1)"""
        return self.min_stack[-1]

# Test
min_stack = MinStack()
min_stack.push(-2)
min_stack.push(0)
min_stack.push(-3)
print(min_stack.get_min())  # -3
min_stack.pop()
print(min_stack.top())      # 0
print(min_stack.get_min())  # -2
```

### 3. Evaluate Reverse Polish Notation

```python
def eval_rpn(tokens: list[str]) -> int:
    """
    Evaluate postfix expression.
    
    Time: O(n)
    Space: O(n)
    """
    stack = []
    operators = {'+', '-', '*', '/'}
    
    for token in tokens:
        if token not in operators:
            stack.append(int(token))
        else:
            b = stack.pop()
            a = stack.pop()
            
            if token == '+':
                stack.append(a + b)
            elif token == '-':
                stack.append(a - b)
            elif token == '*':
                stack.append(a * b)
            elif token == '/':
                stack.append(int(a / b))  # Truncate toward zero
    
    return stack[0]

# Example:
# Input: ["2","1","+","3","*"]
# Output: 9  # ((2 + 1) * 3)
```

### 4. Daily Temperatures

```python
def daily_temperatures(temperatures: list[int]) -> list[int]:
    """
    Find days until warmer temperature using monotonic stack.
    
    Time: O(n)
    Space: O(n)
    """
    n = len(temperatures)
    result = [0] * n
    stack = []  # Store indices
    
    for i in range(n):
        # Pop all days with lower temperature
        while stack and temperatures[stack[-1]] < temperatures[i]:
            prev_day = stack.pop()
            result[prev_day] = i - prev_day
        stack.append(i)
    
    return result

# Example:
# Input: [73,74,75,71,69,72,76,73]
# Output: [1,1,4,2,1,1,0,0]
```

### 5. Largest Rectangle in Histogram

```python
def largest_rectangle_area(heights: list[int]) -> int:
    """
    Find largest rectangle using monotonic stack.
    
    Time: O(n)
    Space: O(n)
    """
    stack = []
    max_area = 0
    heights.append(0)  # Add sentinel
    
    for i in range(len(heights)):
        # Maintain increasing stack
        while stack and heights[stack[-1]] > heights[i]:
            h = heights[stack.pop()]
            w = i if not stack else i - stack[-1] - 1
            max_area = max(max_area, h * w)
        stack.append(i)
    
    heights.pop()  # Remove sentinel
    return max_area

# Example:
# Input: [2,1,5,6,2,3]
# Output: 10  # Rectangle with height 5 and width 2
```

### 6. Simplify Path

```python
def simplify_path(path: str) -> str:
    """
    Simplify Unix-style file path.
    
    Time: O(n)
    Space: O(n)
    """
    stack = []
    
    for component in path.split('/'):
        if component == '..' and stack:
            stack.pop()
        elif component and component not in ['.', '..']:
            stack.append(component)
    
    return '/' + '/'.join(stack)

# Examples:
# "/home/" -> "/home"
# "/../" -> "/"
# "/home//foo/" -> "/home/foo"
# "/a/./b/../../c/" -> "/c"
```

### 7. Decode String

```python
def decode_string(s: str) -> str:
    """
    Decode encoded string.
    
    Time: O(n)
    Space: O(n)
    """
    stack = []
    current_num = 0
    current_str = ""
    
    for char in s:
        if char.isdigit():
            current_num = current_num * 10 + int(char)
        elif char == '[':
            # Push current state
            stack.append((current_str, current_num))
            current_str = ""
            current_num = 0
        elif char == ']':
            # Pop and decode
            prev_str, num = stack.pop()
            current_str = prev_str + current_str * num
        else:
            current_str += char
    
    return current_str

# Examples:
# "3[a]2[bc]" -> "aaabcbc"
# "3[a2[c]]" -> "accaccacc"
# "2[abc]3[cd]ef" -> "abcabccdcdcdef"
```

### 8. Remove K Digits

```python
def remove_k_digits(num: str, k: int) -> str:
    """
    Remove k digits to get smallest number.
    
    Time: O(n)
    Space: O(n)
    """
    stack = []
    
    for digit in num:
        # Remove larger digits
        while k > 0 and stack and stack[-1] > digit:
            stack.pop()
            k -= 1
        stack.append(digit)
    
    # Remove remaining k digits from end
    stack = stack[:-k] if k > 0 else stack
    
    # Convert to string, remove leading zeros
    result = ''.join(stack).lstrip('0')
    return result if result else '0'

# Examples:
# num = "1432219", k = 3 -> "1219"
# num = "10200", k = 1 -> "200"
# num = "10", k = 2 -> "0"
```

### 9. Basic Calculator II

```python
def calculate(s: str) -> int:
    """
    Evaluate expression with +, -, *, /.
    
    Time: O(n)
    Space: O(n)
    """
    stack = []
    num = 0
    operation = '+'
    
    for i, char in enumerate(s):
        if char.isdigit():
            num = num * 10 + int(char)
        
        if char in '+-*/' or i == len(s) - 1:
            if operation == '+':
                stack.append(num)
            elif operation == '-':
                stack.append(-num)
            elif operation == '*':
                stack.append(stack.pop() * num)
            elif operation == '/':
                stack.append(int(stack.pop() / num))
            
            if char in '+-*/':
                operation = char
            num = 0
    
    return sum(stack)

# Example:
# "3+2*2" -> 7
# " 3/2 " -> 1
# " 3+5 / 2 " -> 5
```

### 10. Trapping Rain Water (Stack Solution)

```python
def trap(height: list[int]) -> int:
    """
    Calculate trapped rain water using stack.
    
    Time: O(n)
    Space: O(n)
    """
    stack = []
    water = 0
    
    for i in range(len(height)):
        # Current bar is taller than stack top
        while stack and height[i] > height[stack[-1]]:
            top = stack.pop()
            
            if not stack:
                break
            
            # Calculate water width and height
            distance = i - stack[-1] - 1
            bounded_height = min(height[i], height[stack[-1]]) - height[top]
            water += distance * bounded_height
        
        stack.append(i)
    
    return water

# Example:
# Input: [0,1,0,2,1,0,1,3,2,1,2,1]
# Output: 6
```

---

## Interview Tips

### 1. When to Use Stack

```python
# Use stack for:
- Matching/validating pairs (brackets, tags)
- Reversing order
- Tracking previous elements
- Monotonic sequences
- Expression evaluation
- Backtracking
- DFS traversal
```

### 2. Monotonic Stack Pattern

```python
# Next Greater Element pattern
stack = []
for i, val in enumerate(arr):
    while stack and arr[stack[-1]] < val:
        index = stack.pop()
        # Process index
    stack.append(i)
```

### 3. Stack with Min/Max

```python
# Track min/max with auxiliary stack
class MinMaxStack:
    def __init__(self):
        self.stack = []
        self.min_stack = []
        self.max_stack = []
    
    def push(self, val):
        self.stack.append(val)
        
        min_val = min(val, self.min_stack[-1] if self.min_stack else val)
        self.min_stack.append(min_val)
        
        max_val = max(val, self.max_stack[-1] if self.max_stack else val)
        self.max_stack.append(max_val)
```

### 4. Common Mistakes

```python
# ❌ Forgetting to check empty stack
if stack:
    top = stack.pop()

# ✅ Always check before pop/peek
if not stack:
    return default_value

# ❌ Using stack for problems that need random access
# ✅ Use array/list for random access

# ❌ Not handling edge cases
# ✅ Test with empty input, single element
```

---

## Practice Problems

### Easy
1. [Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)
2. [Implement Stack using Queues](https://leetcode.com/problems/implement-stack-using-queues/)
3. [Baseball Game](https://leetcode.com/problems/baseball-game/)
4. [Backspace String Compare](https://leetcode.com/problems/backspace-string-compare/)
5. [Remove Outermost Parentheses](https://leetcode.com/problems/remove-outermost-parentheses/)

### Medium
1. [Min Stack](https://leetcode.com/problems/min-stack/)
2. [Daily Temperatures](https://leetcode.com/problems/daily-temperatures/)
3. [Evaluate Reverse Polish Notation](https://leetcode.com/problems/evaluate-reverse-polish-notation/)
4. [Decode String](https://leetcode.com/problems/decode-string/)
5. [Asteroid Collision](https://leetcode.com/problems/asteroid-collision/)
6. [Simplify Path](https://leetcode.com/problems/simplify-path/)
7. [Remove K Digits](https://leetcode.com/problems/remove-k-digits/)

### Hard
1. [Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/)
2. [Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/)
3. [Basic Calculator](https://leetcode.com/problems/basic-calculator/)
4. [Maximal Rectangle](https://leetcode.com/problems/maximal-rectangle/)

---

## Summary

### Key Takeaways
- ✅ Stack = LIFO (Last In, First Out)
- ✅ O(1) push, pop, peek operations
- ✅ Perfect for backtracking and reversing
- ✅ Monotonic stack for next greater/smaller
- ✅ Use for expression evaluation

### Common Patterns
```python
# 1. Basic stack operations
stack = []
stack.append(item)    # push
stack.pop()           # pop
stack[-1]             # peek

# 2. Monotonic stack
while stack and condition:
    stack.pop()
stack.append(current)

# 3. Bracket matching
if char in closing:
    if not stack or stack[-1] != pairs[char]:
        return False
    stack.pop()
else:
    stack.append(char)
```

---

**Next**: [Queues →](../05-queues/README.md)

**Happy Coding! 🚀**
