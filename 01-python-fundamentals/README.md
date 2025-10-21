# 🐍 Python Fundamentals for DSA

> Master the Python essentials needed for data structures and algorithms

**Why This Chapter Matters**: Before you can solve complex algorithm problems, you need to be fluent in Python itself. This chapter covers all the Python-specific knowledge you'll need for coding interviews. We're not teaching general programming—we're focusing on the Python features, idioms, and tricks that make solving DSA problems easier and faster.

**What You'll Learn**: You'll master Python's built-in data structures (lists, dicts, sets), understand when to use each one, learn powerful features like list comprehensions and lambda functions, and discover Python's collections module that provides specialized data structures. By the end, you'll write code that's not just correct, but clean and Pythonic.

**How to Use This Chapter**: If you're new to Python, read everything sequentially and practice the examples. If you already know Python, skim for the DSA-specific tips (marked with 💡) and focus on the Collections Module and Useful Libraries sections—these are often overlooked but incredibly powerful for interviews.

---

## 📚 Table of Contents

1. [Python Basics](#python-basics)
2. [Data Types & Operations](#data-types--operations)
3. [Control Flow](#control-flow)
4. [Functions](#functions)
5. [List Comprehensions](#list-comprehensions)
6. [Object-Oriented Programming](#object-oriented-programming)
7. [Python Built-in Functions](#python-built-in-functions)
8. [Collections Module](#collections-module)
9. [Useful Libraries](#useful-libraries)
10. [Common Python Idioms](#common-python-idioms)

---

## Python Basics

**Understanding Python's Core Syntax**

Python is designed to be readable and concise. Unlike languages like Java or C++, Python uses indentation (whitespace) instead of braces to define code blocks. This makes code cleaner but means indentation matters—mixing tabs and spaces will cause errors!

**Dynamic Typing**: Python figures out variable types automatically. You don't declare types explicitly (unlike Java's `int x = 10`). This makes code faster to write but means you need to be careful about what type a variable actually is.

### Variables & Basic Operations

**How Python Variables Work**: In Python, variables are references to objects, not containers that hold values. When you write `x = 10`, you're creating an integer object with value 10 and making `x` point to it. This is different from C/C++ where variables are memory locations.

```python
# Variables (dynamic typing - no type declaration needed)
x = 10          # x is an integer
name = "Python" # name is a string
is_active = True  # is_active is a boolean

# Python figures out types automatically!
# No need for: int x = 10; (like in Java/C++)

# Multiple assignment - assign multiple variables in one line
a, b, c = 1, 2, 3  # a=1, b=2, c=3

# 💡 DSA Trick: Swapping without temporary variable
# In most languages: temp = a; a = b; b = temp
# In Python: just swap in one line!
a, b = b, a  # Swaps values of a and b

# Augmented assignment - shorter way to update variables
count = 0
count += 1  # Same as: count = count + 1
# Also works with: -=, *=, /=, //=, %=, **=
```

**Why This Matters for DSA**: 
- Swapping is common in sorting algorithms
- Multiple assignment makes code cleaner when returning multiple values
- Augmented assignment is faster and more readable in loops

### Print & Input

**Displaying Output and Reading Input**

The `print()` function is your debugging best friend. In interviews, use it to trace variable values and understand what your code is doing. F-strings (formatted string literals) are the modern, readable way to format output.

```python
# Printing - display output to console
print("Hello, World!")  # Basic string output

# F-strings (Python 3.6+) - modern, readable formatting
x = 42
print(f"Value: {x}")  # Output: Value: 42
print(f"Calculation: {x * 2}")  # Can include expressions: Calculation: 84

# 💡 DSA Debugging Trick: Use f-strings to trace execution
# print(f"left={left}, right={right}, mid={mid}")

# Custom separator
print("Multiple", "values", sep="-")  # Output: Multiple-values-separated

# Input - reading from user (not used in online judges, but good to know)
name = input("Enter name: ")  # Returns string
number = int(input("Enter number: "))  # Convert to integer

# 💡 Interview Note: Most online coding platforms don't need input()
# They pass test cases as function arguments instead
```

---

## Data Types & Operations

**Python's Built-in Data Structures**

Python provides powerful built-in data types that would require libraries in other languages. Understanding which data type to use is crucial for efficient algorithms. Each has different time complexities for operations like access, search, insert, and delete.

**The Big Four for DSA**:
1. **Lists** - Dynamic arrays, O(1) access by index, O(n) search
2. **Dictionaries** - Hash maps, O(1) average lookup, perfect for caching
3. **Sets** - Unordered collections, O(1) membership testing
4. **Strings** - Immutable character sequences, special methods for text processing

### Numbers

**Python's Number Types and Operations**

Python handles three types of numbers: integers (whole numbers), floats (decimals), and complex numbers (rarely used in DSA). Unlike C/C++, Python integers have unlimited precision—you can work with numbers as large as your memory allows!

```python
# Integer - whole numbers with unlimited precision
x = 10
y = -5
big_num = 10**100  # Python handles arbitrary precision

# Float
pi = 3.14159
scientific = 1.5e-3  # 0.0015

# Complex
z = 3 + 4j

# Operations
result = x + y      # Addition
result = x - y      # Subtraction
result = x * y      # Multiplication
result = x / y      # Division (float)
result = x // y     # Floor division
result = x % y      # Modulo
result = x ** y     # Exponentiation

# Useful functions
abs(-10)            # 10
pow(2, 3)           # 8
max(1, 2, 3)        # 3
min(1, 2, 3)        # 1
divmod(10, 3)       # (3, 1) - quotient and remainder
```

### Strings

**Working with Text in Python**

Strings are one of the most important data types for coding interviews. About 20-30% of interview problems involve string manipulation. Python strings are **immutable** (can't be changed after creation), which affects how you solve problems.

**Key Concept - Immutability**: When you "modify" a string, Python actually creates a new string. This means operations like `s += "x"` in a loop are O(n²) because each concatenation creates a new string and copies all characters!

**String vs List**: Since strings are immutable, convert to a list when you need to modify individual characters: `chars = list(s)`, modify it, then convert back: `result = "".join(chars)`.

```python
# Creation - three ways to create strings
s = "Hello"      # Double quotes
s = 'Hello'      # Single quotes (same thing)
s = """Multi     # Triple quotes for multi-line strings
line             # Preserves line breaks
string"""

# 💡 DSA Tip: Use """ for clarity when building test cases

# Indexing (0-based, like arrays)
first = s[0]        # 'H' - first character
last = s[-1]        # 'o' - last character (negative = from end)
second_last = s[-2] # 'l' - second from end

# 💡 Interview Trick: s[-1] is cleaner than s[len(s)-1]

# Slicing [start:end:step] - extracts substring
# Format: s[start:end:step]
# - start: inclusive
# - end: exclusive  
# - step: increment (default 1)

substring = s[1:4]  # 'ell' (indices 1,2,3 - not 4!)
from_start = s[:3]  # 'Hel' (start from beginning)
to_end = s[2:]      # 'llo' (go to end)
reverse = s[::-1]   # 'olleH' (step=-1 reverses!)

# 💡 DSA Must-Know: s[::-1] is the fastest way to reverse a string
# Common in palindrome problems!

# String methods
s.lower()           # 'hello'
s.upper()           # 'HELLO'
s.strip()           # Remove whitespace
s.split()           # Split into list
"-".join(['a','b']) # 'a-b'
s.replace('l', 'L') # 'HeLLo'
s.find('e')         # 1 (index, -1 if not found)
s.count('l')        # 2
s.startswith('He')  # True
s.endswith('lo')    # True

# String formatting
name = "Alice"
age = 25
f"Name: {name}, Age: {age}"  # f-strings (preferred)
"Name: {}, Age: {}".format(name, age)
```

### Lists (Dynamic Arrays)

```python
# Creation
nums = [1, 2, 3, 4, 5]
empty = []
mixed = [1, "two", 3.0, [4, 5]]

# Accessing
first = nums[0]     # 1
last = nums[-1]     # 5
subset = nums[1:3]  # [2, 3]

# Modifying
nums[0] = 10        # [10, 2, 3, 4, 5]
nums.append(6)      # Add to end: [10, 2, 3, 4, 5, 6]
nums.insert(0, 0)   # Insert at index: [0, 10, 2, 3, 4, 5, 6]
nums.extend([7, 8]) # Add multiple: [0, 10, 2, 3, 4, 5, 6, 7, 8]
nums.remove(10)     # Remove first occurrence
popped = nums.pop() # Remove and return last element
nums.pop(0)         # Remove and return element at index

# Operations
len(nums)           # Length
sum(nums)           # Sum of elements
min(nums)           # Minimum
max(nums)           # Maximum
nums.sort()         # Sort in-place
sorted(nums)        # Return sorted copy
nums.reverse()      # Reverse in-place
nums.count(2)       # Count occurrences
nums.index(3)       # Find index (raises error if not found)

# List concatenation
list1 + list2       # Combine lists
list1 * 3           # Repeat list

# Checking membership
3 in nums           # True/False
```

### Tuples (Immutable Lists)

```python
# Creation
point = (1, 2)
single = (1,)       # Note the comma
empty = ()

# Unpacking
x, y = point        # x=1, y=2

# Tuples are immutable
# point[0] = 5      # Error!

# Use cases: return multiple values, dictionary keys
def get_coordinates():
    return (10, 20)

x, y = get_coordinates()
```

### Sets

```python
# Creation
nums = {1, 2, 3, 4}
empty = set()       # {} creates empty dict, not set!

# Operations
nums.add(5)         # Add element
nums.remove(2)      # Remove (error if not found)
nums.discard(2)     # Remove (no error if not found)
nums.pop()          # Remove arbitrary element

# Set operations
a = {1, 2, 3}
b = {3, 4, 5}

a | b               # Union: {1, 2, 3, 4, 5}
a & b               # Intersection: {3}
a - b               # Difference: {1, 2}
a ^ b               # Symmetric difference: {1, 2, 4, 5}

# Checking membership (O(1) average)
3 in a              # True
```

### Dictionaries (Hash Maps)

```python
# Creation
person = {"name": "Alice", "age": 25}
empty = {}
from_pairs = dict([("a", 1), ("b", 2)])

# Accessing
name = person["name"]           # "Alice" (KeyError if not found)
name = person.get("name")       # "Alice" (None if not found)
age = person.get("age", 0)      # Default value

# Modifying
person["city"] = "NYC"          # Add/update
del person["age"]               # Delete
removed = person.pop("name")    # Remove and return

# Dictionary operations
person.keys()                   # dict_keys(['name', 'city'])
person.values()                 # dict_values(['Alice', 'NYC'])
person.items()                  # dict_items([('name', 'Alice'), ...])

# Checking membership (O(1) average)
"name" in person                # True

# Dictionary comprehension
squares = {x: x**2 for x in range(5)}
```

---

## Control Flow

### If-Else Statements

```python
x = 10

if x > 0:
    print("Positive")
elif x < 0:
    print("Negative")
else:
    print("Zero")

# Ternary operator
result = "Even" if x % 2 == 0 else "Odd"

# Multiple conditions
if x > 0 and x < 10:
    print("Single digit positive")

if x == 0 or x == 1:
    print("Zero or one")

if not x:
    print("x is falsy")
```

### Loops

```python
# For loop
for i in range(5):          # 0 to 4
    print(i)

for i in range(2, 8):       # 2 to 7
    print(i)

for i in range(0, 10, 2):   # 0, 2, 4, 6, 8 (step=2)
    print(i)

# Iterate over list
nums = [1, 2, 3, 4, 5]
for num in nums:
    print(num)

# Enumerate (get index and value)
for i, num in enumerate(nums):
    print(f"Index {i}: {num}")

for i, num in enumerate(nums, start=1):  # Start from 1
    print(f"Position {i}: {num}")

# Iterate over dictionary
person = {"name": "Alice", "age": 25}
for key in person:
    print(key, person[key])

for key, value in person.items():
    print(key, value)

# While loop
count = 0
while count < 5:
    print(count)
    count += 1

# Loop control
for i in range(10):
    if i == 3:
        continue    # Skip to next iteration
    if i == 7:
        break       # Exit loop
    print(i)

# For-else (executes if no break)
for i in range(5):
    if i == 10:
        break
else:
    print("Loop completed without break")
```

---

## Functions

### Basic Functions

```python
def greet(name):
    """Print a greeting message."""
    print(f"Hello, {name}!")

greet("Alice")

# Return value
def add(a, b):
    return a + b

result = add(5, 3)  # 8

# Multiple return values (tuple)
def get_stats(nums):
    return min(nums), max(nums), sum(nums) / len(nums)

minimum, maximum, average = get_stats([1, 2, 3, 4, 5])
```

### Default Arguments

```python
def power(base, exponent=2):
    return base ** exponent

power(3)        # 9 (uses default exponent=2)
power(3, 3)     # 27
```

### Keyword Arguments

```python
def describe_pet(animal, name):
    print(f"I have a {animal} named {name}")

describe_pet("dog", "Buddy")
describe_pet(name="Buddy", animal="dog")  # Order doesn't matter
```

### Variable Arguments

```python
# *args - variable positional arguments
def sum_all(*args):
    return sum(args)

sum_all(1, 2, 3, 4, 5)  # 15

# **kwargs - variable keyword arguments
def print_info(**kwargs):
    for key, value in kwargs.items():
        print(f"{key}: {value}")

print_info(name="Alice", age=25, city="NYC")
```

### Lambda Functions

```python
# Anonymous functions
square = lambda x: x ** 2
square(5)  # 25

add = lambda x, y: x + y
add(3, 4)  # 7

# Often used with map, filter, sorted
nums = [1, 2, 3, 4, 5]
squares = list(map(lambda x: x**2, nums))
evens = list(filter(lambda x: x % 2 == 0, nums))
```

### Type Hints (Python 3.5+)

```python
def add(a: int, b: int) -> int:
    return a + b

def process_list(items: list[int]) -> list[int]:
    return [x * 2 for x in items]

from typing import Optional, Union, Dict, List

def find_element(arr: List[int], target: int) -> Optional[int]:
    """Returns index or None."""
    try:
        return arr.index(target)
    except ValueError:
        return None
```

---

## List Comprehensions

### Basic List Comprehension

```python
# Traditional way
squares = []
for i in range(10):
    squares.append(i ** 2)

# List comprehension (more Pythonic)
squares = [i ** 2 for i in range(10)]

# With condition
evens = [i for i in range(10) if i % 2 == 0]

# Transform list
names = ["alice", "bob", "charlie"]
upper_names = [name.upper() for name in names]
```

### Nested List Comprehension

```python
# 2D list (matrix)
matrix = [[i * j for j in range(3)] for i in range(3)]
# [[0, 0, 0], [0, 1, 2], [0, 2, 4]]

# Flatten 2D list
matrix = [[1, 2], [3, 4], [5, 6]]
flat = [num for row in matrix for num in row]
# [1, 2, 3, 4, 5, 6]
```

### Set & Dict Comprehension

```python
# Set comprehension
unique_squares = {x**2 for x in range(-5, 6)}

# Dict comprehension
square_dict = {x: x**2 for x in range(5)}
# {0: 0, 1: 1, 2: 4, 3: 9, 4: 16}
```

### Generator Expressions

```python
# Similar to list comprehension but lazy evaluation
gen = (x**2 for x in range(10))  # Generator object
squares = list(gen)  # Convert to list

# Memory efficient for large data
sum_of_squares = sum(x**2 for x in range(1000000))
```

---

## Object-Oriented Programming

### Classes

```python
class Node:
    """Node for linked list."""
    
    def __init__(self, value):
        self.value = value
        self.next = None
    
    def __repr__(self):
        return f"Node({self.value})"

node = Node(10)
print(node)  # Node(10)
```

### Class with Methods

```python
class Stack:
    def __init__(self):
        self.items = []
    
    def push(self, item):
        self.items.append(item)
    
    def pop(self):
        if not self.is_empty():
            return self.items.pop()
        return None
    
    def peek(self):
        if not self.is_empty():
            return self.items[-1]
        return None
    
    def is_empty(self):
        return len(self.items) == 0
    
    def size(self):
        return len(self.items)
    
    def __len__(self):
        """Allow len(stack) to work."""
        return len(self.items)
    
    def __repr__(self):
        return f"Stack({self.items})"

stack = Stack()
stack.push(1)
stack.push(2)
print(len(stack))  # 2
```

### Inheritance

```python
class Animal:
    def __init__(self, name):
        self.name = name
    
    def speak(self):
        pass

class Dog(Animal):
    def speak(self):
        return f"{self.name} says Woof!"

class Cat(Animal):
    def speak(self):
        return f"{self.name} says Meow!"

dog = Dog("Buddy")
print(dog.speak())  # Buddy says Woof!
```

---

## Python Built-in Functions

### Essential Functions for DSA

```python
# Type conversion
int("123")          # 123
float("3.14")       # 3.14
str(123)            # "123"
list("abc")         # ['a', 'b', 'c']
tuple([1, 2, 3])    # (1, 2, 3)
set([1, 2, 2, 3])   # {1, 2, 3}

# Math functions
abs(-10)            # 10
pow(2, 3)           # 8
round(3.7)          # 4
divmod(10, 3)       # (3, 1)

# Sequence functions
len([1, 2, 3])      # 3
sum([1, 2, 3])      # 6
min([1, 2, 3])      # 1
max([1, 2, 3])      # 3
sorted([3, 1, 2])   # [1, 2, 3]
reversed([1, 2, 3]) # Iterator

# Iteration functions
enumerate(['a', 'b', 'c'])  # (0, 'a'), (1, 'b'), (2, 'c')
zip([1, 2], ['a', 'b'])     # (1, 'a'), (2, 'b')
map(str, [1, 2, 3])         # Iterator of strings
filter(lambda x: x > 0, [-1, 0, 1, 2])  # Iterator of positive

# all() and any()
all([True, True, False])    # False
any([True, False, False])   # True

# range
range(5)            # 0, 1, 2, 3, 4
range(2, 8)         # 2, 3, 4, 5, 6, 7
range(0, 10, 2)     # 0, 2, 4, 6, 8
```

---

## Collections Module

### Counter

```python
from collections import Counter

# Count frequency
nums = [1, 1, 2, 3, 3, 3]
count = Counter(nums)   # Counter({3: 3, 1: 2, 2: 1})

# Most common
count.most_common(2)    # [(3, 3), (1, 2)]

# String character count
s = "hello"
Counter(s)              # Counter({'l': 2, 'h': 1, 'e': 1, 'o': 1})
```

### defaultdict

```python
from collections import defaultdict

# Automatically initialize missing keys
graph = defaultdict(list)
graph['A'].append('B')  # No KeyError

# Group items
words = ['apple', 'ant', 'ball', 'bat']
grouped = defaultdict(list)
for word in words:
    grouped[word[0]].append(word)
# {'a': ['apple', 'ant'], 'b': ['ball', 'bat']}
```

### deque (Double-ended queue)

```python
from collections import deque

# Efficient for queue operations
dq = deque([1, 2, 3])

dq.append(4)        # Add to right: [1, 2, 3, 4]
dq.appendleft(0)    # Add to left: [0, 1, 2, 3, 4]
dq.pop()            # Remove from right: 4
dq.popleft()        # Remove from left: 0

# Use as queue (FIFO)
queue = deque()
queue.append(1)     # Enqueue
queue.popleft()     # Dequeue
```

### OrderedDict (Maintains insertion order)

```python
from collections import OrderedDict

# Regular dict (Python 3.7+) maintains order too
# But OrderedDict has additional methods

od = OrderedDict()
od['a'] = 1
od['b'] = 2
od['c'] = 3

od.move_to_end('a')     # Move 'a' to end
od.popitem(last=False)  # Remove first item
```

### namedtuple

```python
from collections import namedtuple

# Create tuple with named fields
Point = namedtuple('Point', ['x', 'y'])
p = Point(10, 20)

p.x         # 10
p.y         # 20
p[0]        # 10 (also accessible by index)
```

---

## Useful Libraries

### heapq (Min Heap)

```python
import heapq

# Create heap
nums = [3, 1, 4, 1, 5, 9, 2, 6]
heapq.heapify(nums)  # Convert to heap in-place

# Push and pop
heapq.heappush(nums, 0)  # Add element
smallest = heapq.heappop(nums)  # Remove smallest

# Get n smallest/largest
heapq.nsmallest(3, nums)  # [0, 1, 1]
heapq.nlargest(3, nums)   # [9, 6, 5]

# Max heap (negate values)
max_heap = [-x for x in nums]
heapq.heapify(max_heap)
```

### bisect (Binary Search)

```python
import bisect

# Find insertion point
nums = [1, 3, 5, 7, 9]
bisect.bisect_left(nums, 5)   # 2 (leftmost position)
bisect.bisect_right(nums, 5)  # 3 (rightmost position)

# Insert maintaining order
bisect.insort(nums, 6)  # [1, 3, 5, 6, 7, 9]
```

### itertools

```python
import itertools

# Combinations
list(itertools.combinations([1, 2, 3], 2))
# [(1, 2), (1, 3), (2, 3)]

# Permutations
list(itertools.permutations([1, 2, 3], 2))
# [(1, 2), (1, 3), (2, 1), (2, 3), (3, 1), (3, 2)]

# Product (Cartesian product)
list(itertools.product([1, 2], ['a', 'b']))
# [(1, 'a'), (1, 'b'), (2, 'a'), (2, 'b')]

# Accumulate (cumulative sum)
list(itertools.accumulate([1, 2, 3, 4]))
# [1, 3, 6, 10]

# Chain (flatten)
list(itertools.chain([1, 2], [3, 4], [5]))
# [1, 2, 3, 4, 5]
```

---

## Common Python Idioms

### Checking Truthiness

```python
# Empty collections are falsy
if not my_list:        # Better than len(my_list) == 0
    print("Empty list")

if my_dict:            # Check if dict has items
    print("Non-empty dict")
```

### Iteration Tricks

```python
# Iterate with index
for i, val in enumerate(nums):
    print(i, val)

# Iterate over pairs
for a, b in zip(list1, list2):
    print(a, b)

# Iterate in reverse
for i in reversed(range(10)):
    print(i)

# Pairwise iteration
for i in range(len(nums) - 1):
    curr, next = nums[i], nums[i + 1]
```

### Swapping & Multiple Assignment

```python
# Swap variables
a, b = b, a

# Multiple assignment
x, y, z = 1, 2, 3

# Unpacking
first, *middle, last = [1, 2, 3, 4, 5]
# first=1, middle=[2, 3, 4], last=5
```

### Default Values

```python
# Get dict value with default
value = my_dict.get(key, default_value)

# Set default if key doesn't exist
my_dict.setdefault(key, [])
my_dict[key].append(value)
```

### Chaining Comparisons

```python
# Check if x is in range [0, 10]
if 0 <= x <= 10:
    print("In range")
```

---

## 🎯 Practice Problems

Test your Python knowledge with these problems:

1. **Fibonacci**: Write a function to generate first n Fibonacci numbers
2. **Palindrome**: Check if a string is a palindrome
3. **Anagram**: Check if two strings are anagrams
4. **List Operations**: Find second largest element in a list
5. **Dictionary**: Group words by their first letter
6. **Set Operations**: Find common elements in multiple lists
7. **String Manipulation**: Reverse words in a sentence
8. **List Comprehension**: Generate list of prime numbers up to n

### Solutions

See [python-basics-exercises.md](./python-basics-exercises.md) for detailed solutions.

---

## 📚 Next Steps

Once comfortable with Python fundamentals:
1. Move to [Complexity Analysis](../02-complexity-analysis/README.md)
2. Start learning [Data Structures](../03-data-structures/README.md)
3. Practice basic problems on [LeetCode](https://leetcode.com/)

---

**Happy Coding! 🚀**
