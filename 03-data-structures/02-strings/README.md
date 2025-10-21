# 🔤 Strings - Python DSA

> Master string manipulation - one of the most tested topics in interviews

**Why Strings Matter**: String problems appear in 30-40% of coding interviews, second only to arrays. They test your understanding of immutability, efficiency (avoiding O(n²) string concatenation), pattern recognition (sliding window, two pointers), and algorithms (KMP, Rabin-Karp). Companies love string questions because they're practical—every application processes text!

**What You'll Learn**: How Python strings actually work under the hood, why immutability matters for complexity, essential string manipulation patterns (palindromes, anagrams, substrings), powerful algorithms for pattern matching, and the string-specific tricks that separate good candidates from great ones.

**Real-World Impact**: String algorithms power autocomplete, spell checkers, DNA sequencing, plagiarism detection, search engines, and data validation. Mastering strings prepares you for real engineering work, not just interviews.

---

## 📚 Table of Contents

1. [Introduction](#introduction)
2. [Theory & Concepts](#theory--concepts)
3. [Python String Operations](#python-string-operations)
4. [Common Patterns](#common-patterns)
5. [String Algorithms](#string-algorithms)
6. [Classic Problems](#classic-problems)
7. [Interview Tips](#interview-tips)
8. [Practice Problems](#practice-problems)

---

## Introduction

**What Are Strings?**

A string is a sequence of characters—letters, digits, symbols, spaces. Think of it as an array of characters, but with special properties and methods for text processing. In Python, strings are **immutable**, which profoundly affects how you write efficient string code.

**Strings** are sequences of characters. In Python, strings are **immutable**, meaning they cannot be changed after creation. This is THE most important thing to understand about Python strings—it affects performance and how you solve problems.

### Key Characteristics

**Understanding these characteristics is crucial for interview success:**

- ✅ **Immutable** - Cannot modify in place
  - *Why it matters*: Every "modification" creates a new string
  - *Performance impact*: `s += "x"` in a loop is O(n²), not O(n)!
  - *Interview trap*: Building strings incorrectly is a common mistake
  - *Solution*: Use list of chars, modify, then join: O(n)

- ✅ **Indexed** - Access by position O(1)
  - *How it works*: Just like arrays, strings are contiguous in memory
  - *Usage*: `s[0]` is instant, whether string has 10 or 10 million characters
  - *Negative indexing*: `s[-1]` for last character (very Pythonic!)

- ✅ **Iterable** - Can loop through characters
  - *Common pattern*: `for char in string:` processes each character
  - *With enumerate*: `for i, char in enumerate(string):` gets index too
  - *List comprehension*: `[char.upper() for char in string]`

- ✅ **Unicode support** - Python 3 strings are Unicode by default
  - *Global applications*: Handles any language: English, Chinese, Arabic, Emoji 😊
  - *Interview note*: Usually not tested, but good to know for system design

- ❌ **Not mutable** - String operations create new strings
  - *Critical understanding*: This is why certain algorithms need different approaches for strings vs arrays
  - *Memory consideration*: Creating many intermediate strings wastes memory

### When Strings Appear in Interviews

**Expect string questions for**:
- **Text processing and parsing**: Validating input, extracting data
  - Example: "Parse log files", "Valid parentheses"
  
- **Pattern matching and searching**: Finding substrings efficiently
  - Example: "Implement strStr()", "Find all anagrams"
  
- **Data validation**: Email, phone numbers, custom formats
  - Example: "Valid palindrome", "Valid number"
  
- **Palindrome checks**: Forward = backward reading
  - Example: "Longest palindromic substring", "Valid palindrome"
  
- **Anagram detection**: Same letters, different order
  - Example: "Group anagrams", "Valid anagram"
  
- **Substring problems**: Contiguous or non-contiguous portions
  - Example: "Longest substring without repeating characters"
  
- **Almost every coding interview!**: Seriously, practice strings!
  - Companies love them because they're practical and test multiple skills

---

## Theory & Concepts

### String Immutability

**THE Most Important String Concept for Interviews**

Understanding immutability isn't just academic—it directly affects whether your solution is O(n) or O(n²). This alone can make the difference between passing and failing an interview.

**What Immutability Means**:
```python
# Strings are immutable - cannot change characters in place
s = "hello"
# s[0] = 'H'  # TypeError! Cannot modify

# Every "modification" creates a NEW string
s = 'H' + s[1:]  # "Hello"
# Memory: "hello" still exists, "Hello" is new string
# Old string gets garbage collected eventually
```

**The O(n²) Trap** (Most Common String Interview Mistake!):
```python
# ❌ WRONG: This is O(n²)!
def build_string_wrong(n):
    s = ""
    for i in range(n):
        s += str(i)  # Creates new string each time!
    return s

# Why O(n²)?
# Iteration 1: s = "" + "0" = "0"           (copy 0 chars)
# Iteration 2: s = "0" + "1" = "01"         (copy 1 char)
# Iteration 3: s = "01" + "2" = "012"       (copy 2 chars)
# ...
# Iteration n: s = "012...n-2" + "n-1"      (copy n-1 chars)
# Total: 0 + 1 + 2 + ... + (n-1) = n(n-1)/2 = O(n²)

# ✅ CORRECT: This is O(n)
def build_string_correct(n):
    parts = []  # List is mutable!
    for i in range(n):
        parts.append(str(i))  # Append is O(1)
    return ''.join(parts)  # Join is O(n), total = O(n)

# 💡 Interview Rule: Never concatenate strings in a loop!
# Always use list + join for O(n) instead of O(n²)
```

**When Immutability Helps**:
```python
# Strings can be dictionary keys (because immutable = hashable)
word_count = {}
word_count["hello"] = 5  # ✅ Works!

# Lists can't be dict keys (mutable = not hashable)
# char_list = ['h', 'e', 'l', 'l', 'o']
# word_count[char_list] = 5  # ❌ TypeError!

# Strings are thread-safe (immutability = no race conditions)
# Multiple threads can safely read same string
```

### String vs List: When to Convert

**Critical Decision**: Should you work with string or convert to list?

```python
# String: Immutable sequence of characters
s = "abc"
# s[0] = 'x'  # ❌ Error! Can't modify

# List: Mutable sequence of characters
chars = list(s)  # ['a', 'b', 'c']
chars[0] = 'x'   # ✅ OK! ['x', 'b', 'c']
s = ''.join(chars)  # Convert back: "xbc"
```

**When to Convert to List**:
1. **Need to modify many characters**: Convert once, modify O(n), join once
   ```python
   # Reverse string by swapping
   chars = list(s)
   left, right = 0, len(chars) - 1
   while left < right:
       chars[left], chars[right] = chars[right], chars[left]
       left += 1
       right -= 1
   result = ''.join(chars)  # Total: O(n)
   ```

2. **Building complex strings**: Append to list, join at end
   ```python
   # Build string with many modifications
   parts = []
   for item in data:
       parts.append(str(item))
       parts.append(",")
   result = ''.join(parts)  # Efficient!
   ```

**When to Keep as String**:
1. **Just reading/analyzing**: No need to convert
   ```python
   # Count vowels - just read, don't modify
   vowels = set('aeiouAEIOU')
   count = sum(1 for char in s if char in vowels)
   ```

2. **Using string methods**: Built-in methods are optimized
   ```python
   # String methods are fast - use them!
   s.lower(), s.strip(), s.split()
   ```

**💡 Conversion Costs**:
- `list(s)`: O(n) time, O(n) space
- `''.join(chars)`: O(n) time, O(n) space
- Only convert if you need O(n) modifications
- Don't convert for just 1-2 changes (slicing is fine)

---

## Python String Operations

**Mastering Python's Rich String API**

Python provides incredibly powerful string methods that would require custom code in other languages. Knowing these well makes your interview code cleaner and faster to write.

### Creation

**Different Ways to Create Strings**

```python
# Method 1: Single or double quotes (identical)
s1 = 'hello'  # Single quotes
s2 = "hello"  # Double quotes
# Use whichever doesn't require escaping:
# "It's nice" better than 'It\'s nice'
# 'She said "hi"' better than "She said \"hi\""

# Method 2: Triple quotes (multiline strings)
s3 = """This is
a multiline
string"""
# Preserves line breaks and indentation
# Useful for: docstrings, SQL queries, formatted text

# Method 3: Raw strings (ignore escape sequences)
path = r'C:\Users\name\documents'
# Without r: 'C:\\Users\\name\\documents' (need to escape \)
# With r: treats backslash as literal character
# 💡 Interview use: regex patterns, file paths

# Method 4: f-strings (formatted string literals) - Python 3.6+
name = "Alice"
age = 25
message = f"{name} is {age} years old"
# Output: "Alice is 25 years old"

# Can include expressions!
print(f"Next year: {age + 1}")  # "Next year: 26"
print(f"Name upper: {name.upper()}")  # "Name upper: ALICE"

# Older formatting (avoid in new code):
"Hello %s" % name  # C-style
"Hello {}".format(name)  # .format() method
```

### Indexing & Slicing

**Accessing Parts of Strings**

```python
s = "Python"

# Indexing - O(1) access by position
first = s[0]      # 'P' - first character (index 0)
second = s[1]     # 'y' - second character
last = s[-1]      # 'n' - last character (negative = from end)
second_last = s[-2]  # 'o' - second from end

# 💡 Interview Tip: s[-1] is cleaner than s[len(s)-1]

# Slicing [start:end:step] - creates new string
# Format: s[start:end:step]
# - start: inclusive (default 0)
# - end: exclusive (default len(s))
# - step: increment (default 1)

s[1:4]           # 'yth' - characters at indices 1, 2, 3 (not 4!)
s[:3]            # 'Pyt' - first 3 characters (omit start = from 0)
s[3:]            # 'hon' - from index 3 to end (omit end = to len)
s[::2]           # 'Pto' - every 2nd character (0, 2, 4)
s[::-1]          # 'nohtyP' - REVERSE! (step=-1)

# Why reverse works:
# step=-1 means "go backwards"
# omitted start/end with negative step = from end to beginning

# Slicing is forgiving - never raises IndexError!
s[100:]          # '' - empty string (start beyond end)
s[10:20]         # '' - entire range invalid
s[-100:]         # 'Python' - negative beyond start = from beginning

# 💡 Common interview patterns:
# - Reverse string: s[::-1]
# - Remove first char: s[1:]
# - Remove last char: s[:-1]
# - First half: s[:len(s)//2]
# - Second half: s[len(s)//2:]
```

### Common Methods

**Essential String Methods (You MUST Know These!)**

```python
s = "Hello World"

# ===== CASE CONVERSION =====
# All create NEW strings (remember: immutable!)

s.lower()              # 'hello world' - all lowercase
s.upper()              # 'HELLO WORLD' - all uppercase
s.capitalize()         # 'Hello world' - first letter capital, rest lower
s.title()              # 'Hello World' - title case (first letter of each word)
s.swapcase()           # 'hELLO wORLD' - swap case of each letter

# 💡 Interview use: case-insensitive comparisons
# if s1.lower() == s2.lower():  # Compare ignoring case

# ===== SEARCHING (Finding substrings) =====

s.find('o')            # 4 - index of first 'o'
                       # Returns -1 if not found (no exception!)
s.find('o', 5)         # 7 - find 'o' starting from index 5
s.find('xyz')          # -1 - not found

s.index('o')           # 4 - same as find()
                       # Raises ValueError if not found!
                       # 💡 Use find() in interviews (safer)

s.rfind('o')           # 7 - REVERSE find - last occurrence
s.count('l')           # 3 - count occurrences of 'l'

# 💡 Interview pattern: Check before using index
if 'pattern' in s:  # Check membership first
    idx = s.find('pattern')  # Then find safely

# ===== CHECKING / VALIDATION =====
# All return True/False

s.startswith('Hello')  # True - starts with "Hello"?
s.startswith('Hi')     # False
s.endswith('World')    # True - ends with "World"?
s.endswith('world')    # False (case-sensitive!)

# Character type checks
'abc'.isalpha()        # True - all letters?
'123'.isdigit()        # True - all digits?
'abc123'.isalnum()     # True - all letters/digits?
'   '.isspace()        # True - all whitespace?
'abc'.islower()        # True - all lowercase letters?
'ABC'.isupper()        # True - all uppercase letters?

# 💡 Interview use: input validation
def is_valid_username(s):
    return s.isalnum() and 3 <= len(s) <= 20

# ===== WHITESPACE HANDLING =====

'  hello  '.strip()     # 'hello' - remove leading/trailing whitespace
'  hello  '.lstrip()    # 'hello  ' - remove leading only
'  hello  '.rstrip()    # '  hello' - remove trailing only

# Can strip specific characters!
'***hello***'.strip('*')  # 'hello' - remove * from ends
'www.example.com'.lstrip('w.')  # 'example.com'

# 💡 Interview use: cleaning input
user_input = input().strip()  # Remove accidental spaces

# ===== REPLACING (Creates new string!) =====

s.replace('World', 'Python')  # 'Hello Python' - replace all occurrences
s.replace('l', 'L')           # 'HeLLo WorLd' - replaces ALL 'l'
s.replace('l', 'L', 1)        # 'HeLlo World' - replace only FIRST occurrence

# 💡 Interview use: multiple replacements (careful with order!)
text = "abc"
text = text.replace('a', 'b').replace('b', 'c')
# Result: 'ccc' NOT 'bcc' - first replace creates more 'b's!

# ===== SPLITTING (String → List) =====
# One of THE most important operations in interviews!

sentence = "apple,banana,orange"

# split() - by delimiter (default: any whitespace)
sentence.split(',')        # ['apple', 'banana', 'orange']
"a  b   c".split()         # ['a', 'b', 'c'] - splits on ANY whitespace
"a  b   c".split(' ')      # ['a', '', '', 'b', '', '', '', 'c'] - splits on EACH space
# 💡 Rule: Use split() with no args for whitespace (cleaner!)

# Can limit splits:
"a-b-c-d".split('-', 1)    # ['a', 'b-c-d'] - split ONCE
"a-b-c-d".split('-', 2)    # ['a', 'b', 'c-d'] - split TWICE

# rsplit() - like split() but from the right
"a-b-c-d".rsplit('-', 1)   # ['a-b-c', 'd'] - split once from RIGHT

# splitlines() - split on newlines
text = "line1\nline2\nline3"
text.splitlines()          # ['line1', 'line2', 'line3']
# Better than split('\n') - handles \r\n, \r, \n

# partition() - split into 3 parts: before, sep, after
email = "user@example.com"
user, at, domain = email.partition('@')
# user = 'user', at = '@', domain = 'example.com'
# If separator not found: returns (string, '', '')

# 💡 Interview pattern: parse structured data
def parse_time(s):
    # "14:30:45" → (14, 30, 45)
    return tuple(map(int, s.split(':')))

# ===== JOINING (List → String) =====
# THE correct way to build strings from parts!

words = ['Hello', 'World', 'from', 'Python']

' '.join(words)            # 'Hello World from Python'
', '.join(words)           # 'Hello, World, from, Python'
''.join(words)             # 'HelloWorldfromPython'
'-'.join(words)            # 'Hello-World-from-Python'

# Works with ANY iterable!
''.join(['a', 'b', 'c'])   # 'abc'
''.join(('x', 'y', 'z'))   # 'xyz'
','.join(str(x) for x in [1,2,3])  # '1,2,3'

# ✅ CORRECT pattern for building strings:
parts = []
for i in range(1000):
    parts.append(str(i))
result = ''.join(parts)    # O(n) - efficient!

# ❌ WRONG pattern (O(n²) - we covered this earlier!):
result = ''
for i in range(1000):
    result += str(i)       # Creates new string each iteration!

# 💡 Interview rule: Always use join() to build strings from parts

# ===== PADDING & ALIGNMENT =====

'42'.zfill(5)              # '00042' - zero-fill to width 5
                           # Respects sign: '-42'.zfill(5) → '-0042'

'hello'.center(10)         # '  hello   ' - center in width 10
'hello'.ljust(10)          # 'hello     ' - left-justify
'hello'.rjust(10)          # '     hello' - right-justify

# Can specify fill character:
'hello'.center(10, '*')    # '**hello***'
'hello'.ljust(10, '-')     # 'hello-----'

# 💡 Interview use: formatting output
def format_table(rows):
    # Make aligned columns
    return [row.ljust(20) for row in rows]

# ===== TESTING CONTENT =====

# Check if string contains only certain characters
'abc123'.isalnum()         # True - letters + digits only
'abc_123'.isalnum()        # False - underscore not allowed!

'123'.isnumeric()          # True - more lenient than isdigit()
'½'.isnumeric()            # True - accepts fractions, superscripts
'½'.isdigit()              # False

'hello world'.isascii()    # True - all ASCII characters?
'hello 世界'.isascii()      # False - contains non-ASCII

# 💡 Interview use: validate identifiers
def is_valid_variable_name(s):
    return s.isidentifier()  # Valid Python variable name?
    # Rules: start with letter/_, then letters/digits/_

is_valid_variable_name('my_var')   # True
is_valid_variable_name('2var')     # False - starts with digit
is_valid_variable_name('my-var')   # False - hyphen not allowed
```

**Key Takeaways for Interviews:**

1. **Remember Immutability**: All methods return NEW strings
   ```python
   s = "hello"
   s.upper()  # Returns "HELLO" but doesn't modify s!
   print(s)   # Still "hello"
   
   s = s.upper()  # Need to reassign to save result
   ```

2. **split() and join() are Your Friends**:
   - `split()` → process → `join()` is the standard pattern
   - Always use `join()` to build strings from lists

3. **find() vs index()**:
   - Use `find()` - returns -1 if not found
   - Avoid `index()` - raises exception (more code to handle)

4. **in operator is Often Best**:
   ```python
   # Simple and readable:
   if 'pattern' in string:
       # ...
   
   # More verbose:
   if string.find('pattern') != -1:
       # ...
   ```

5. **Method Chaining Works**:
   ```python
   # Can chain multiple methods:
   result = text.strip().lower().replace('  ', ' ')
   # But don't go crazy - keep it readable!
   ```
```

### Character Operations

**Working with Individual Characters**

Understanding character-level operations is crucial for many string problems.

```python
# ===== CHARACTER TYPE CHECKS =====

char = 'A'

# Basic checks (work on single characters)
char.isalpha()         # True - is it a letter?
char.isdigit()         # False - is it 0-9?
char.isalnum()         # True - is it letter OR digit?
char.isupper()         # True - is it uppercase letter?
char.islower()         # False - is it lowercase letter?
char.isspace()         # False - is it whitespace?

# Examples for each type:
'5'.isdigit()          # True
'_'.isalnum()          # False - underscore is neither!
' '.isspace()          # True
'\n'.isspace()         # True - newline counts
'\t'.isspace()         # True - tab counts

# 💡 Interview use: filtering characters
def get_alphanumeric_only(s):
    # Remove all non-alphanumeric characters
    return ''.join(c for c in s if c.isalnum())

# ===== ASCII VALUES & CONVERSIONS =====

# ord() - character to ASCII value
ord('A')               # 65 - uppercase A
ord('a')               # 97 - lowercase a
ord('0')               # 48 - digit zero
ord(' ')               # 32 - space

# chr() - ASCII value to character
chr(65)                # 'A'
chr(97)                # 'a'
chr(48)                # '0'

# 💡 Interview use cases:

# 1. Case conversion (manual way - usually just use .upper()/.lower())
def to_upper(c):
    if 'a' <= c <= 'z':
        return chr(ord(c) - 32)  # a=97, A=65, difference=32
    return c

# 2. Shift characters (Caesar cipher)
def shift_char(c, shift):
    if c.isalpha():
        # Handle wrap-around: z + 1 = a
        base = ord('A') if c.isupper() else ord('a')
        shifted = (ord(c) - base + shift) % 26
        return chr(base + shifted)
    return c

print(shift_char('a', 1))   # 'b'
print(shift_char('z', 1))   # 'a' - wraps around

# 3. Character distance (for anagrams, etc.)
def char_distance(c1, c2):
    return abs(ord(c1) - ord(c2))

print(char_distance('a', 'd'))  # 3

# ===== COMMON CHARACTER PATTERNS =====

# Check if character is vowel
def is_vowel(c):
    return c.lower() in 'aeiou'

# Check if character is consonant
def is_consonant(c):
    return c.isalpha() and not is_vowel(c)

# Check if character is alphanumeric for palindrome check
def is_alnum_lower(c):
    # Common pattern: convert to lowercase & check if alphanumeric
    return c.lower() if c.isalnum() else None

# ===== KEY FACTS TO REMEMBER =====

# 1. Uppercase letters: A-Z are ASCII 65-90
# 2. Lowercase letters: a-z are ASCII 97-122
# 3. Digits: 0-9 are ASCII 48-57
# 4. Difference between upper & lower: always 32
#    ord('a') - ord('A') = 97 - 65 = 32

# 5. Alphabetical distance:
#    ord('d') - ord('a') = 3  # 'd' is 3 positions after 'a'

# 💡 Interview Tip: For character frequency problems
# Array indexing is faster than dictionary!
def char_to_index(c):
    # Convert 'a'-'z' to 0-25
    return ord(c.lower()) - ord('a')

# Use in frequency array:
freq = [0] * 26  # One slot for each letter
for c in "hello":
    if c.isalpha():
        freq[char_to_index(c)] += 1
# freq[7] = 1 (h), freq[4] = 1 (e), freq[11] = 2 (l), freq[14] = 1 (o)
```

**When to Use ord() vs Built-in Methods:**

✅ **Use Built-in Methods** (usually better):
- Case conversion: Use `.upper()`, `.lower()`
- Character checks: Use `.isalpha()`, `.isdigit()`, etc.
- They're optimized and more readable!

✅ **Use ord()** (when you need it):
- Character shifting (Caesar cipher)
- Character distance calculations
- Array indexing by character ('a' → 0, 'b' → 1, etc.)
- Custom character manipulations

---

## Common Patterns

**Master These String Patterns for Interviews**

These patterns appear in 80%+ of string interview questions. Understanding them deeply will make you solve problems faster and write cleaner code.

---

### Pattern 1: Two Pointers (Palindrome Check)

**When to Use:**
- Comparing characters from both ends
- Palindrome problems
- In-place reversal or modifications
- Finding pairs/patterns with symmetry

**Why It Works:**
- Compare elements moving toward center
- O(1) space - no extra data structure needed
- Can skip unwanted characters efficiently

**Template:**
```python
def two_pointers_template(s):
    left, right = 0, len(s) - 1
    
    while left < right:
        # Skip unwanted characters from left
        while left < right and should_skip(s[left]):
            left += 1
        
        # Skip unwanted characters from right
        while left < right and should_skip(s[right]):
            right -= 1
        
        # Process current pair
        if not valid_pair(s[left], s[right]):
            return False
        
        left += 1
        right -= 1
    
    return True
```

**Detailed Example: Valid Palindrome**

```python
def is_palindrome(s: str) -> bool:
    """
    Check if string is palindrome (ignore non-alphanumeric, case-insensitive).
    
    Example: "A man, a plan, a canal: Panama" → True
    
    Why this approach:
    - Two pointers avoid creating cleaned string (saves space)
    - Skip non-alphanumeric on-the-fly (single pass)
    - Case-insensitive by converting during comparison
    
    Time: O(n) - single pass through string
    Space: O(1) - only two pointers
    """
    left, right = 0, len(s) - 1
    
    while left < right:
        # Skip non-alphanumeric from left
        # Why: "A man, a plan" → compare 'A' with 'n', not 'A' with ' '
        while left < right and not s[left].isalnum():
            left += 1
        
        # Skip non-alphanumeric from right
        while left < right and not s[right].isalnum():
            right -= 1
        
        # Compare characters (case-insensitive)
        # Why .lower(): 'A' should equal 'a' for palindrome
        if s[left].lower() != s[right].lower():
            return False  # Found mismatch - not palindrome
        
        left += 1
        right -= 1
    
    return True  # All characters matched

# Visual walkthrough: "A man, a plan, a canal: Panama"
# 
# Step 1: left='A'(0), right='a'(29)
#         Compare 'a' == 'a' ✓
# 
# Step 2: left='m'(2), right='m'(27)
#         Skip spaces at (1, 28)
#         Compare 'm' == 'm' ✓
# 
# Step 3: left='a'(3), right='a'(26)
#         Compare 'a' == 'a' ✓
# 
# Continue until left >= right...
# Result: True

# Test cases
print(is_palindrome("A man, a plan, a canal: Panama"))  # True
print(is_palindrome("race a car"))  # False
print(is_palindrome(""))  # True - empty is palindrome
print(is_palindrome("a"))  # True - single char is palindrome
```

**Common Variations:**

1. **Check if string is palindrome (alphanumeric only)** - shown above
2. **Valid palindrome ignoring specific characters**:
   ```python
   def is_palindrome_ignore(s, ignore_chars):
       left, right = 0, len(s) - 1
       while left < right:
           while left < right and s[left] in ignore_chars:
               left += 1
           while left < right and s[right] in ignore_chars:
               right -= 1
           if s[left] != s[right]:
               return False
           left += 1
           right -= 1
       return True
   ```

3. **Count palindromic substrings** - use expand around center (Pattern 5)

4. **Reverse string in-place** (if using list):
   ```python
   def reverse_string(chars):
       left, right = 0, len(chars) - 1
       while left < right:
           chars[left], chars[right] = chars[right], chars[left]
           left += 1
           right -= 1
   ```

💡 **Interview Tips:**
- Ask: "Should I consider case?" "Special characters?"
- Two-pointer avoids creating filtered string (better space)
- Watch for `left < right` in inner while loops (prevents crossing)

---

### Pattern 2: Sliding Window (Longest Substring)

**When to Use:**
- Find longest/shortest substring with property
- Substrings with unique characters
- Substrings containing specific characters
- Variable-length substring problems

**Why It Works:**
- Maintains a "window" of valid characters
- Expand window (move right) to explore
- Shrink window (move left) when invalid
- Avoids checking all O(n²) substrings

**How It's Better Than Brute Force:**
```python
# ❌ Brute Force: O(n³)
# Check every substring - start at each position, try all lengths
for i in range(n):           # O(n) start positions
    for j in range(i, n):     # O(n) end positions
        if has_unique(s[i:j+1]):  # O(n) to check uniqueness
            # ...
# Total: O(n) * O(n) * O(n) = O(n³)

# ✅ Sliding Window: O(n)
# Maintain set, expand/shrink window
# Each character processed at most twice (added once, removed once)
# Total: O(2n) = O(n)
```

**Template:**
```python
def sliding_window_template(s):
    left = 0
    window_data = {}  # Track window state (set, dict, etc.)
    result = 0
    
    for right in range(len(s)):
        # Add s[right] to window
        window_data[s[right]] = window_data.get(s[right], 0) + 1
        
        # Shrink window while invalid
        while window_invalid(window_data):
            window_data[s[left]] -= 1
            left += 1
        
        # Update result with current valid window
        result = max(result, right - left + 1)
    
    return result
```

**Detailed Example: Longest Substring Without Repeating Characters**

```python
def length_of_longest_substring(s: str) -> int:
    """
    Find length of longest substring without repeating characters.
    
    Example: "abcabcbb" → 3 (substring "abc")
    
    Why sliding window:
    - Brute force: Check all O(n²) substrings = O(n³) with uniqueness check
    - Sliding window: Each char added/removed once = O(n)
    
    How it works:
    1. Expand window by moving right pointer
    2. If duplicate found, shrink from left until no duplicates
    3. Track maximum window size seen
    
    Time: O(n) - each character visited at most twice (by left & right)
    Space: O(min(n, m)) - where m is charset size (26 for lowercase letters)
    """
    char_set = set()  # Track characters in current window
    left = 0          # Left boundary of window
    max_length = 0    # Maximum window size found
    
    # Expand window with right pointer
    for right in range(len(s)):
        # Shrink window until s[right] can be added without duplicate
        # Why while: might need to remove multiple characters
        while s[right] in char_set:
            char_set.remove(s[left])  # Remove leftmost char
            left += 1                  # Move left boundary
        
        # Now s[right] not in set - add it
        char_set.add(s[right])
        
        # Update max length
        # Window size = right - left + 1
        max_length = max(max_length, right - left + 1)
    
    return max_length

# Visual walkthrough: "abcabcbb"
#
# right=0, s[right]='a'
#   char_set={}, left=0
#   Add 'a' → char_set={'a'}, window="a", max_length=1
#
# right=1, s[right]='b'
#   char_set={'a'}, left=0
#   Add 'b' → char_set={'a','b'}, window="ab", max_length=2
#
# right=2, s[right]='c'
#   char_set={'a','b'}, left=0
#   Add 'c' → char_set={'a','b','c'}, window="abc", max_length=3
#
# right=3, s[right]='a'  ← DUPLICATE!
#   char_set={'a','b','c'}, left=0
#   'a' in set! Remove s[0]='a' → char_set={'b','c'}, left=1
#   'a' not in set now! Add 'a' → char_set={'b','c','a'}
#   window="bca", max_length=3 (no change)
#
# right=4, s[right]='b'  ← DUPLICATE!
#   char_set={'b','c','a'}, left=1
#   'b' in set! Remove s[1]='b' → char_set={'c','a'}, left=2
#   'b' not in set now! Add 'b' → char_set={'c','a','b'}
#   window="cab", max_length=3 (no change)
#
# Continue... Result: 3

# Test cases
print(length_of_longest_substring("abcabcbb"))  # 3 ("abc")
print(length_of_longest_substring("bbbbb"))     # 1 ("b")
print(length_of_longest_substring("pwwkew"))    # 3 ("wke")
print(length_of_longest_substring(""))          # 0 (empty)
print(length_of_longest_substring("au"))        # 2 ("au")
```

**Common Variations:**

1. **Longest substring with at most K distinct characters**:
   ```python
   def length_of_longest_substring_k_distinct(s, k):
       char_count = {}
       left = 0
       max_length = 0
       
       for right in range(len(s)):
           # Add character to window
           char_count[s[right]] = char_count.get(s[right], 0) + 1
           
           # Shrink while more than k distinct characters
           while len(char_count) > k:
               char_count[s[left]] -= 1
               if char_count[s[left]] == 0:
                   del char_count[s[left]]
               left += 1
           
           max_length = max(max_length, right - left + 1)
       
       return max_length
   ```

2. **Longest substring with at most K replacements** (Leetcode 424)

3. **Minimum window substring** (Leetcode 76) - more advanced

💡 **Interview Tips:**
- Use set for "unique characters" problems
- Use dict/Counter for "count/frequency" problems
- Window size = `right - left + 1`
- Shrink with `while` not `if` (might need multiple removals)
- left pointer never goes backward (monotonic)

---

### Pattern 3: Hash Map / Frequency Counter (Anagram Check)

**When to Use:**
- Anagram problems
- Character frequency counting
- Finding matching patterns
- Grouping strings by character composition

**Why It Works:**
- Anagrams have same character frequencies
- Hash map/Counter makes frequency comparison O(1) per lookup
- Can compare entire freq distributions in O(n)

**How It's Better:**
```python
# ❌ Sorting approach: O(n log n)
def is_anagram_sort(s, t):
    return sorted(s) == sorted(t)  # O(n log n) sorting

# ✅ Frequency counting: O(n)
def is_anagram_count(s, t):
    from collections import Counter
    return Counter(s) == Counter(t)  # O(n) counting
```

**Template:**
```python
from collections import Counter

def frequency_pattern(strings):
    # Count character frequencies
    freq = Counter(string)
    
    # Or manually:
    freq = {}
    for char in string:
        freq[char] = freq.get(char, 0) + 1
    
    # Use frequencies for comparison/grouping
    return freq
```

**Detailed Example: Valid Anagram**

```python
def is_anagram(s: str, t: str) -> bool:
    """
    Check if two strings are anagrams.
    
    Example: "anagram", "nagaram" → True
    Example: "rat", "car" → False
    
    What's an anagram?
    - Same characters, same frequencies, different arrangement
    - "listen" and "silent" are anagrams
    - "abc" and "bca" are anagrams
    - "abc" and "abcc" are NOT (different frequencies)
    
    Why Counter is perfect:
    - Counts frequency of each character
    - Two Counters equal if same chars with same counts
    - O(n) time to build, O(1) per comparison
    
    Time: O(n + m) where n, m are lengths of s, t
    Space: O(1) if only lowercase letters (max 26 chars), else O(n)
    """
    # Quick check: different lengths can't be anagrams
    if len(s) != len(t):
        return False
    
    from collections import Counter
    return Counter(s) == Counter(t)

# How Counter works internally:
# Counter("hello") → {'h': 1, 'e': 1, 'l': 2, 'o': 1}
# Counter("olleh") → {'o': 1, 'l': 2, 'e': 1, 'h': 1}
# These are equal! (dict comparison)

# Alternative: Manual counting (same logic, more code)
def is_anagram_manual(s: str, t: str) -> bool:
    """
    Same algorithm without Counter - good to know for interviews
    where imports might not be allowed.
    """
    if len(s) != len(t):
        return False
    
    count = {}
    
    # Count characters in s (increment)
    for char in s:
        count[char] = count.get(char, 0) + 1
    
    # Decrease count for characters in t
    for char in t:
        if char not in count:
            return False  # Character in t but not in s
        count[char] -= 1
        if count[char] < 0:
            return False  # More occurrences in t than s
    
    # All counts should be exactly 0
    # (guaranteed by length check and decrement logic)
    return True

# Alternative: Array counting for lowercase letters only
def is_anagram_array(s: str, t: str) -> bool:
    """
    Fastest approach for lowercase letters only.
    Uses fixed-size array instead of dictionary.
    
    Why faster:
    - Array access O(1), no hashing needed
    - Better cache locality
    - But only works for known character set!
    """
    if len(s) != len(t):
        return False
    
    # Array for 26 lowercase letters
    count = [0] * 26
    
    # Increment for s, decrement for t
    for i in range(len(s)):
        count[ord(s[i]) - ord('a')] += 1
        count[ord(t[i]) - ord('a')] -= 1
    
    # All should be 0 if anagram
    return all(c == 0 for c in count)

# Visual walkthrough: "abc", "bca"
#
# Using Counter:
#   Counter("abc") = {'a': 1, 'b': 1, 'c': 1}
#   Counter("bca") = {'b': 1, 'c': 1, 'a': 1}
#   Equal? YES → True
#
# Using manual counting:
#   After "abc": count = {'a': 1, 'b': 1, 'c': 1}
#   Process 'b': count = {'a': 1, 'b': 0, 'c': 1}
#   Process 'c': count = {'a': 1, 'b': 0, 'c': 0}
#   Process 'a': count = {'a': 0, 'b': 0, 'c': 0}
#   All zero? YES → True
#
# Using array counting:
#   count[0] (for 'a'): +1 (from s), -1 (from t) = 0
#   count[1] (for 'b'): +1 (from s), -1 (from t) = 0
#   count[2] (for 'c'): +1 (from s), -1 (from t) = 0
#   All zero? YES → True

# Test cases
print(is_anagram("anagram", "nagaram"))  # True
print(is_anagram("rat", "car"))          # False
print(is_anagram("", ""))                # True - empty strings
print(is_anagram("a", "a"))              # True
print(is_anagram("a", "b"))              # False
```

**Common Variations:**

1. **Group Anagrams** (Very common interview question!):
   ```python
   from collections import defaultdict
   
   def group_anagrams(strs):
       """
       Group strings that are anagrams of each other.
       
       Input: ["eat","tea","tan","ate","nat","bat"]
       Output: [["eat","tea","ate"],["tan","nat"],["bat"]]
       
       Key insight: Anagrams have same sorted string OR same frequency
       """
       # Approach 1: Use sorted string as key
       groups = defaultdict(list)
       for s in strs:
           key = ''.join(sorted(s))  # "eat" → "aet"
           groups[key].append(s)
       return list(groups.values())
       
       # Approach 2: Use frequency tuple as key (faster!)
       groups = defaultdict(list)
       for s in strs:
           count = [0] * 26
           for c in s:
               count[ord(c) - ord('a')] += 1
           key = tuple(count)  # (1,0,0,1,1,...) - immutable!
           groups[key].append(s)
       return list(groups.values())
   ```

2. **Find All Anagrams in String** (Sliding Window + Frequency!):
   ```python
   def find_anagrams(s, p):
       """Find all start indices of p's anagrams in s."""
       from collections import Counter
       
       p_count = Counter(p)
       window_count = Counter()
       result = []
       
       for i in range(len(s)):
           # Add character to window
           window_count[s[i]] += 1
           
           # Remove character from left if window too large
           if i >= len(p):
               if window_count[s[i - len(p)]] == 1:
                   del window_count[s[i - len(p)]]
               else:
                   window_count[s[i - len(p)]] -= 1
           
           # Check if current window is anagram
           if window_count == p_count:
               result.append(i - len(p) + 1)
       
       return result
   ```

3. **Valid Anagram with frequency limit**:
   ```python
   def can_construct(ransomNote, magazine):
       """Can we construct ransomNote using letters from magazine?"""
       from collections import Counter
       
       note_count = Counter(ransomNote)
       mag_count = Counter(magazine)
       
       # Check if magazine has enough of each letter
       for char, count in note_count.items():
           if mag_count[char] < count:
               return False
       return True
       
       # One-liner version:
       # return not (Counter(ransomNote) - Counter(magazine))
   ```

💡 **Interview Tips:**
- **Counter vs dict**: Counter easier to read, dict is manual
- **Array vs dict**: Array faster for known charset (a-z), dict for any chars
- **sorted() approach**: Simpler code but O(n log n) instead of O(n)
- **Ask about character set**: Lowercase only? Unicode? Affects space complexity
- **Tuple trick**: Use `tuple(count)` as dict key (lists aren't hashable!)

**When to use each approach:**
- ✅ **Counter**: Clean, Pythonic, handles any characters
- ✅ **Array**: Fastest for a-z only, interview favorite
- ✅ **Manual dict**: When Counter not allowed/available
- ✅ **sorted()**: Quick solution for small strings

---

### Pattern 4: String Builder / Efficient Construction

**When to Use:**
- Building strings from many parts
- Reversing words/characters with modifications
- Any scenario with repeated concatenation

**Why It Matters:**
We covered this in depth earlier - string concatenation in loops is O(n²)!

```python
# ❌ WRONG: O(n²) - creates new string each iteration
result = ""
for i in range(n):
    result += str(i)  # Each += copies entire string!

# ✅ CORRECT: O(n) - list append is O(1), join is O(n)
parts = []
for i in range(n):
    parts.append(str(i))
result = ''.join(parts)
```

**Template:**
```python
def string_builder_template(items):
    # Build with list
    parts = []
    for item in items:
        parts.append(process(item))
    return ''.join(parts)
    
    # Or use list comprehension (more Pythonic)
    return ''.join(process(item) for item in items)
```

**Detailed Example 1: Reverse Words**

```python
def reverse_words(s: str) -> str:
    """
    Reverse the order of words in a string.
    
    Example: "  hello world  " → "world hello"
    Example: "a good   example" → "example good a"
    
    Why this approach:
    - split() handles multiple spaces automatically
    - reversed() is O(n) generator (doesn't create new list)
    - join() assembles result efficiently
    
    Time: O(n) - split, reverse, join all O(n)
    Space: O(n) - need to store words
    """
    # split() with no args:
    # - Splits on ANY whitespace (space, tab, newline)
    # - Removes empty strings from result
    # - Perfect for this problem!
    words = s.split()
    
    # reversed() returns iterator (lazy evaluation)
    return ' '.join(reversed(words))

# How it works: "  hello world  "
# Step 1: split() → ['hello', 'world']
#         (notice: no empty strings from leading/trailing spaces!)
# Step 2: reversed() → iterator over ['world', 'hello']
# Step 3: join() → "world hello"

# Alternative: Reverse list in place
def reverse_words_v2(s: str) -> str:
    words = s.split()
    words.reverse()  # In-place reversal
    return ' '.join(words)

# Alternative: Pythonic one-liner
def reverse_words_v3(s: str) -> str:
    return ' '.join(s.split()[::-1])

# Test cases
print(reverse_words("  hello world  "))  # "world hello"
print(reverse_words("a good   example"))  # "example good a"
print(reverse_words(""))  # ""
```

**Detailed Example 2: Build String Efficiently**

```python
def compress_string(s: str) -> str:
    """
    Compress string using character counts.
    
    Example: "aabcccccaaa" → "a2b1c5a3"
    
    Why list + join:
    - Many appends (each character might contribute 2 parts: char + count)
    - String concatenation would be O(n²)
    - List append is O(1), join is O(n) → total O(n)
    
    Time: O(n)
    Space: O(n) for result
    """
    if not s:
        return ""
    
    result = []
    count = 1
    
    for i in range(1, len(s)):
        if s[i] == s[i-1]:
            count += 1
        else:
            # Append character and its count
            result.append(s[i-1])
            result.append(str(count))
            count = 1
    
    # Don't forget last group!
    result.append(s[-1])
    result.append(str(count))
    
    # Join all parts
    compressed = ''.join(result)
    
    # Return compressed only if shorter
    return compressed if len(compressed) < len(s) else s

# How it works: "aabcccccaaa"
# i=1: s[1]='a' == s[0]='a', count=2
# i=2: s[2]='b' != s[1]='a', append 'a','2', count=1
# i=3: s[3]='c' != s[2]='b', append 'b','1', count=1
# i=4: s[4]='c' == s[3]='c', count=2
# i=5: s[5]='c' == s[4]='c', count=3
# i=6: s[6]='c' == s[5]='c', count=4
# i=7: s[7]='c' == s[6]='c', count=5
# i=8: s[8]='a' != s[7]='c', append 'c','5', count=1
# i=9: s[9]='a' == s[8]='a', count=2
# i=10: s[10]='a' == s[9]='a', count=3
# End: append 'a','3'
# Result: ['a','2','b','1','c','5','a','3'] → "a2b1c5a3"

print(compress_string("aabcccccaaa"))  # "a2b1c5a3"
print(compress_string("abcdef"))  # "abcdef" (compression not beneficial)
```

**Common Variations:**

1. **Reverse characters in each word** (not word order):
   ```python
   def reverse_chars_in_words(s):
       """'hello world' → 'olleh dlrow'"""
       words = s.split()
       reversed_words = [word[::-1] for word in words]
       return ' '.join(reversed_words)
   ```

2. **Build with conditional logic**:
   ```python
   def remove_vowels(s):
       """Remove all vowels from string."""
       vowels = set('aeiouAEIOU')
       return ''.join(c for c in s if c not in vowels)
   ```

3. **Interleave strings**:
   ```python
   def interleave(s1, s2):
       """'abc', 'def' → 'adbecf'"""
       result = []
       for c1, c2 in zip(s1, s2):
           result.append(c1)
           result.append(c2)
       # Add remaining characters
       result.extend(s1[len(s2):])
       result.extend(s2[len(s1):])
       return ''.join(result)
   ```

💡 **Interview Tips:**
- **Always use list + join** for building strings from parts
- **split() with no args** is smarter than split(' ')
- **Generator expressions** save memory: `''.join(c for c in s)`
- **StringBuilder in other languages**: Java has StringBuilder, C# has StringBuilder, Python uses list!

---

### Pattern 5: Expand Around Center (Palindromes)

**When to Use:**
- Find longest palindromic substring
- Count palindromic substrings
- Problems where you check symmetry around a point

**Why It Works:**
- Palindromes are symmetric around their center
- Check from center outward (expanding)
- Every palindrome has a center (character or gap between chars)
- For string of length n: n centers (odd-length) + n-1 centers (even-length) = 2n-1 possible centers

**Key Insight:**
```python
# Two types of palindromes:
# 1. Odd length: center is a character
#    "aba" - center is 'b' (index 1)
#    "racecar" - center is 'e' (index 3)
#
# 2. Even length: center is between two characters
#    "abba" - center is between two 'b's
#    "noon" - center is between two 'o's

# Must check BOTH for each position!
```

**Template:**
```python
def expand_around_center_template(s):
    def expand(left, right):
        # Expand while characters match and in bounds
        while left >= 0 and right < len(s) and s[left] == s[right]:
            # Process current palindrome
            left -= 1
            right += 1
        return left + 1, right - 1  # Return last valid positions
    
    for i in range(len(s)):
        # Check odd-length palindromes (single character center)
        left, right = expand(i, i)
        
        # Check even-length palindromes (center between i and i+1)
        left, right = expand(i, i + 1)
```

**Detailed Example: Longest Palindromic Substring**

```python
def longest_palindrome(s: str) -> str:
    """
    Find the longest palindromic substring.
    
    Example: "babad" → "bab" or "aba" (both valid)
    Example: "cbbd" → "bb"
    
    Why expand around center:
    - Brute force: Check all O(n²) substrings, O(n) each → O(n³)
    - Dynamic Programming: O(n²) time, O(n²) space
    - Expand around center: O(n²) time, O(1) space! ✅
    
    How it works:
    1. For each possible center (2n-1 centers)
    2. Expand outward while characters match
    3. Track longest palindrome found
    
    Time: O(n²) - O(n) centers × O(n) expansion each
    Space: O(1) - only storing indices
    """
    
    def expand_around_center(left: int, right: int) -> int:
        """
        Expand around center and return length of palindrome.
        
        Why pass left AND right:
        - Odd-length: pass (i, i) - single char center
        - Even-length: pass (i, i+1) - gap between chars
        """
        # Expand while in bounds and characters match
        while left >= 0 and right < len(s) and s[left] == s[right]:
            left -= 1   # Move left pointer left
            right += 1  # Move right pointer right
        
        # When loop ends, left and right are ONE PAST valid palindrome
        # Length = right - left - 1
        # Example: if left=0, right=4 after expansion
        #   Last valid was left=1, right=3
        #   Length = 3 - 1 + 1 = 3
        #   Which is (right - 1) - (left + 1) + 1 = right - left - 1
        return right - left - 1
    
    if not s:
        return ""
    
    start = 0  # Start index of longest palindrome
    end = 0    # End index of longest palindrome
    
    # Check each possible center
    for i in range(len(s)):
        # Case 1: Odd-length palindrome (single character center)
        # Example: "aba" centered at 'b'
        len1 = expand_around_center(i, i)
        
        # Case 2: Even-length palindrome (center between two chars)
        # Example: "abba" centered between two 'b's
        len2 = expand_around_center(i, i + 1)
        
        # Take the longer of the two
        max_len = max(len1, len2)
        
        # Update if we found a longer palindrome
        if max_len > end - start:
            # Calculate start and end indices
            # If max_len is odd: center is i, radius is max_len // 2
            # If max_len is even: center is between i and i+1
            start = i - (max_len - 1) // 2
            end = i + max_len // 2
    
    return s[start:end + 1]

# Visual walkthrough: "babad"
#
# i=0, s[0]='b'
#   Odd: expand(0,0) → 'b' → length=1
#   Even: expand(0,1) → no match → length=0
#   max_len=1, current best: s[0:1]="b"
#
# i=1, s[1]='a'
#   Odd: expand(1,1) → 'a' → then (0,2) → 'bab' → length=3
#   Even: expand(1,2) → 'ab' no match → length=0
#   max_len=3, current best: s[0:3]="bab"
#
# i=2, s[2]='b'
#   Odd: expand(2,2) → 'b' → then (1,3) → 'aba' → length=3
#   Even: expand(2,3) → 'ba' no match → length=0
#   max_len=3, no change (same length as current best)
#
# i=3, s[3]='a'
#   Odd: expand(3,3) → 'a' → then (2,4) → 'bad' no → length=1
#   Even: expand(3,4) → 'ad' no match → length=0
#   max_len=1, no change
#
# i=4, s[4]='d'
#   Odd: expand(4,4) → 'd' → length=1
#   Even: expand(4,5) → out of bounds → length=0
#   max_len=1, no change
#
# Result: "bab" (could also return "aba", same length)

# Test cases
print(longest_palindrome("babad"))  # "bab" or "aba"
print(longest_palindrome("cbbd"))   # "bb"
print(longest_palindrome("a"))      # "a"
print(longest_palindrome("ac"))     # "a" or "c"
```

**Common Variations:**

1. **Count all palindromic substrings**:
   ```python
   def count_substrings(s):
       """Count all palindromic substrings."""
       def expand(left, right):
           count = 0
           while left >= 0 and right < len(s) and s[left] == s[right]:
               count += 1  # Found a palindrome!
               left -= 1
               right += 1
           return count
       
       total = 0
       for i in range(len(s)):
           total += expand(i, i)      # Odd-length
           total += expand(i, i + 1)  # Even-length
       return total
   ```

2. **Shortest palindrome by adding characters to front**:
   - Find longest palindrome starting at index 0
   - Add reverse of remaining substring to front

3. **Check if string can form palindrome** (different approach):
   - Use frequency counter
   - At most one character can have odd frequency

💡 **Interview Tips:**
- **Remember 2n-1 centers**: Don't forget even-length palindromes!
- **Helper function**: Makes code cleaner, easier to debug
- **Index calculation**: Practice deriving start/end from center and length
- **Edge cases**: Empty string, single character, all same character

**Why expand-around-center beats DP:**
- DP: O(n²) time, O(n²) space (need 2D table)
- Expand: O(n²) time, O(1) space (just track indices)
- Same time, less space - interview favorite!

---
    """
    if not s:
        return ""
    
    def expand_around_center(left: int, right: int) -> int:
        """Expand while characters match."""
        while left >= 0 and right < len(s) and s[left] == s[right]:
            left -= 1
            right += 1
        return right - left - 1
    
    start, end = 0, 0
    
    for i in range(len(s)):
        # Odd length palindrome (center is one char)
        len1 = expand_around_center(i, i)
        
        # Even length palindrome (center is between two chars)
        len2 = expand_around_center(i, i + 1)
        
        max_len = max(len1, len2)
        
        if max_len > end - start:
            start = i - (max_len - 1) // 2
            end = i + max_len // 2
    
    return s[start:end + 1]

# Test
print(longest_palindrome("babad"))  # "bab" or "aba"
print(longest_palindrome("cbbd"))   # "bb"
```

---

## String Algorithms

### KMP (Knuth-Morris-Pratt) Pattern Matching

```python
def kmp_search(text: str, pattern: str) -> int:
    """
    Find first occurrence of pattern in text using KMP.
    
    Time: O(n + m)
    Space: O(m)
    """
    if not pattern:
        return 0
    
    # Build LPS (Longest Proper Prefix which is also Suffix) array
    def build_lps(pattern: str) -> list[int]:
        lps = [0] * len(pattern)
        length = 0
        i = 1
        
        while i < len(pattern):
            if pattern[i] == pattern[length]:
                length += 1
                lps[i] = length
                i += 1
            else:
                if length != 0:
                    length = lps[length - 1]
                else:
                    lps[i] = 0
                    i += 1
        
        return lps
    
    lps = build_lps(pattern)
    i = j = 0  # i for text, j for pattern
    
    while i < len(text):
        if text[i] == pattern[j]:
            i += 1
            j += 1
        
        if j == len(pattern):
            return i - j  # Found at index i - j
        elif i < len(text) and text[i] != pattern[j]:
            if j != 0:
                j = lps[j - 1]
            else:
                i += 1
    
    return -1  # Not found

# Test
print(kmp_search("hello world", "world"))  # 6
```

### Rabin-Karp (Rolling Hash)

```python
def rabin_karp(text: str, pattern: str) -> int:
    """
    Find pattern in text using rolling hash.
    
    Time: O(n + m) average, O(nm) worst
    Space: O(1)
    """
    n, m = len(text), len(pattern)
    if m > n:
        return -1
    
    # Hash parameters
    d = 256  # Number of characters
    q = 101  # Prime number
    
    h = pow(d, m - 1, q)  # d^(m-1) % q
    p_hash = 0  # Pattern hash
    t_hash = 0  # Text window hash
    
    # Calculate initial hashes
    for i in range(m):
        p_hash = (d * p_hash + ord(pattern[i])) % q
        t_hash = (d * t_hash + ord(text[i])) % q
    
    # Slide pattern over text
    for i in range(n - m + 1):
        # Check if hashes match
        if p_hash == t_hash:
            # Verify character by character
            if text[i:i + m] == pattern:
                return i
        
        # Calculate hash for next window
        if i < n - m:
            t_hash = (d * (t_hash - ord(text[i]) * h) + ord(text[i + m])) % q
            if t_hash < 0:
                t_hash += q
    
    return -1

# Test
print(rabin_karp("hello world", "world"))  # 6
```

---

## Classic Problems

### 1. Valid Anagram

**Problem**: Determine if two strings are anagrams.

```python
def is_anagram(s: str, t: str) -> bool:
    """
    Time: O(n)
    Space: O(1) - at most 26 letters
    """
    from collections import Counter
    return Counter(s) == Counter(t)

# Alternative: sorting
def is_anagram_sort(s: str, t: str) -> bool:
    """Time: O(n log n), Space: O(1)"""
    return sorted(s) == sorted(t)

# Test
print(is_anagram("anagram", "nagaram"))  # True
```

### 2. Group Anagrams

**Problem**: Group strings that are anagrams.

```python
def group_anagrams(strs: list[str]) -> list[list[str]]:
    """
    Time: O(n * k log k) where k is max string length
    Space: O(n * k)
    """
    from collections import defaultdict
    
    anagrams = defaultdict(list)
    
    for s in strs:
        # Use sorted string as key
        key = ''.join(sorted(s))
        anagrams[key].append(s)
    
    return list(anagrams.values())

# Alternative: use character count as key
def group_anagrams_v2(strs: list[str]) -> list[list[str]]:
    """
    Time: O(n * k) where k is max string length
    Space: O(n * k)
    """
    from collections import defaultdict
    
    anagrams = defaultdict(list)
    
    for s in strs:
        # Create count array as key
        count = [0] * 26
        for c in s:
            count[ord(c) - ord('a')] += 1
        anagrams[tuple(count)].append(s)
    
    return list(anagrams.values())

# Test
strs = ["eat", "tea", "tan", "ate", "nat", "bat"]
print(group_anagrams(strs))
# [['eat', 'tea', 'ate'], ['tan', 'nat'], ['bat']]
```

### 3. Longest Common Prefix

**Problem**: Find longest common prefix among array of strings.

```python
def longest_common_prefix(strs: list[str]) -> str:
    """
    Time: O(n * m) where m is length of shortest string
    Space: O(1)
    """
    if not strs:
        return ""
    
    # Use first string as reference
    for i in range(len(strs[0])):
        char = strs[0][i]
        
        # Check if all strings have same character at position i
        for s in strs[1:]:
            if i >= len(s) or s[i] != char:
                return strs[0][:i]
    
    return strs[0]

# Test
print(longest_common_prefix(["flower", "flow", "flight"]))  # "fl"
print(longest_common_prefix(["dog", "racecar", "car"]))     # ""
```

### 4. Valid Parentheses

**Problem**: Check if parentheses are valid.

```python
def is_valid(s: str) -> bool:
    """
    Time: O(n)
    Space: O(n)
    """
    stack = []
    mapping = {')': '(', '}': '{', ']': '['}
    
    for char in s:
        if char in mapping:
            # Closing bracket
            if not stack or stack[-1] != mapping[char]:
                return False
            stack.pop()
        else:
            # Opening bracket
            stack.append(char)
    
    return not stack

# Test
print(is_valid("()"))        # True
print(is_valid("()[]{}"))    # True
print(is_valid("(]"))        # False
print(is_valid("([)]"))      # False
```

### 5. Longest Repeating Character Replacement

**Problem**: Find longest substring with same character after k replacements.

```python
def character_replacement(s: str, k: int) -> int:
    """
    Time: O(n)
    Space: O(1) - at most 26 letters
    """
    from collections import defaultdict
    
    count = defaultdict(int)
    max_count = 0
    left = 0
    result = 0
    
    for right in range(len(s)):
        count[s[right]] += 1
        max_count = max(max_count, count[s[right]])
        
        # If window size - max_count > k, shrink window
        while right - left + 1 - max_count > k:
            count[s[left]] -= 1
            left += 1
        
        result = max(result, right - left + 1)
    
    return result

# Test
print(character_replacement("ABAB", 2))    # 4
print(character_replacement("AABABBA", 1)) # 4
```

### 6. Minimum Window Substring

**Problem**: Find minimum window containing all characters of t.

```python
def min_window(s: str, t: str) -> str:
    """
    Time: O(n + m)
    Space: O(m)
    """
    if not s or not t:
        return ""
    
    from collections import Counter
    
    # Count characters in t
    target_count = Counter(t)
    required = len(target_count)
    
    # Sliding window
    left = 0
    formed = 0
    window_count = {}
    
    # Result: (window length, left, right)
    ans = float('inf'), None, None
    
    for right in range(len(s)):
        char = s[right]
        window_count[char] = window_count.get(char, 0) + 1
        
        # Check if frequency of current char matches target
        if char in target_count and window_count[char] == target_count[char]:
            formed += 1
        
        # Try to shrink window
        while left <= right and formed == required:
            char = s[left]
            
            # Update result
            if right - left + 1 < ans[0]:
                ans = (right - left + 1, left, right)
            
            # Remove leftmost character
            window_count[char] -= 1
            if char in target_count and window_count[char] < target_count[char]:
                formed -= 1
            
            left += 1
    
    return "" if ans[0] == float('inf') else s[ans[1]:ans[2] + 1]

# Test
print(min_window("ADOBECODEBANC", "ABC"))  # "BANC"
```

### 7. Encode and Decode Strings

**Problem**: Design algorithm to encode/decode list of strings.

```python
class Codec:
    """
    Encode and decode strings.
    """
    
    def encode(self, strs: list[str]) -> str:
        """
        Encode list of strings to single string.
        Format: length#string
        
        Time: O(n)
        Space: O(1)
        """
        result = []
        for s in strs:
            result.append(f"{len(s)}#{s}")
        return ''.join(result)
    
    def decode(self, s: str) -> list[str]:
        """
        Decode single string to list of strings.
        
        Time: O(n)
        Space: O(1)
        """
        result = []
        i = 0
        
        while i < len(s):
            # Find delimiter
            j = i
            while s[j] != '#':
                j += 1
            
            # Extract length
            length = int(s[i:j])
            
            # Extract string
            result.append(s[j + 1:j + 1 + length])
            
            # Move to next string
            i = j + 1 + length
        
        return result

# Test
codec = Codec()
strs = ["hello", "world", ""]
encoded = codec.encode(strs)
print(encoded)  # "5#hello5#world0#"
print(codec.decode(encoded))  # ["hello", "world", ""]
```

---

## Interview Tips

### 1. Clarify Requirements
- Case-sensitive?
- Only lowercase/uppercase?
- ASCII or Unicode?
- Empty string valid input?

### 2. Common String Operations

```python
# Check if alphanumeric
char.isalnum()

# Convert case
s.lower(), s.upper()

# Count characters
from collections import Counter
Counter(s)

# Check substring
substring in string  # O(n*m)

# Find all occurrences
def find_all(s, sub):
    start = 0
    while True:
        start = s.find(sub, start)
        if start == -1:
            return
        yield start
        start += 1
```

### 3. Space Optimization
```python
# ❌ Bad: O(n²) for concatenation
s = ""
for i in range(n):
    s += str(i)

# ✅ Good: O(n) using list
chars = []
for i in range(n):
    chars.append(str(i))
s = ''.join(chars)
```

### 4. Common Tricks
```python
# Reverse string
s[::-1]

# Check palindrome
s == s[::-1]

# Remove non-alphanumeric
''.join(c for c in s if c.isalnum())

# Count distinct characters
len(set(s))

# Character frequency
from collections import Counter
Counter(s)
```

---

## Practice Problems

### Easy
1. [Valid Palindrome](https://leetcode.com/problems/valid-palindrome/)
2. [Valid Anagram](https://leetcode.com/problems/valid-anagram/)
3. [First Unique Character](https://leetcode.com/problems/first-unique-character-in-a-string/)
4. [Reverse String](https://leetcode.com/problems/reverse-string/)
5. [Is Subsequence](https://leetcode.com/problems/is-subsequence/)

### Medium
1. [Longest Substring Without Repeating](https://leetcode.com/problems/longest-substring-without-repeating-characters/)
2. [Group Anagrams](https://leetcode.com/problems/group-anagrams/)
3. [Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring/)
4. [Longest Repeating Character Replacement](https://leetcode.com/problems/longest-repeating-character-replacement/)
5. [String to Integer (atoi)](https://leetcode.com/problems/string-to-integer-atoi/)

### Hard
1. [Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/)
2. [Word Ladder](https://leetcode.com/problems/word-ladder/)
3. [Substring with Concatenation](https://leetcode.com/problems/substring-with-concatenation-of-all-words/)

---

## Summary

### Key Takeaways
- ✅ Strings are immutable in Python
- ✅ Two pointers for palindromes
- ✅ Sliding window for substrings
- ✅ Hash map for anagrams/frequency
- ✅ Use list for building strings

### Quick Reference
```python
# Common operations
s.lower(), s.upper()
s.strip(), s.split()
s.replace(old, new)
s.find(sub), s.count(sub)
s.startswith(), s.endswith()
c.isalnum(), c.isalpha()

# Efficient building
chars = []
chars.append(c)
''.join(chars)

# Pattern matching
s in text  # Substring check
```

---

**Next**: [Linked Lists →](../03-linked-lists/README.md)

**Happy Coding! 🚀**
