# 🌲 Tries (Prefix Trees) - Python DSA

> Efficient string search and prefix operations

---

## 📚 Table of Contents

1. [Introduction](#introduction)
2. [Trie Structure](#trie-structure)
3. [Implementation](#implementation)
4. [Common Operations](#common-operations)
5. [Common Patterns](#common-patterns)
6. [Classic Problems](#classic-problems)
7. [Interview Tips](#interview-tips)
8. [Practice Problems](#practice-problems)

---

## Introduction

**Trie** (pronounced "try") is a tree data structure for storing strings where each node represents a character.

### Key Characteristics
- ✅ **O(m) search** - m is string length
- ✅ **O(m) insertion** - Independent of number of strings
- ✅ **Prefix operations** - Efficient autocomplete
- ✅ **Space optimization** - Shared prefixes
- ❌ **Memory overhead** - Pointers for each character

### Real-World Examples
- 🔍 Autocomplete/search suggestions
- 📖 Dictionary implementation
- 🌐 IP routing tables
- 📝 Spell checkers
- 🎮 Word games (Boggle, Scrabble)

### Trie Operations

| Operation | Time | Description |
|-----------|------|-------------|
| Insert | O(m) | Add word |
| Search | O(m) | Find exact word |
| StartsWith | O(m) | Find prefix |
| Delete | O(m) | Remove word |

*m = length of word*

---

## Trie Structure

### Visual Representation

```
Insert: "cat", "car", "dog"

        root
       /    \
      c      d
      |      |
      a      o
     / \     |
    t   r    g
   *    *    *

* = end of word
```

### Node Structure

```python
class TrieNode:
    def __init__(self):
        self.children = {}  # character → TrieNode
        self.is_end = False  # marks end of word
```

---

## Implementation

### Basic Trie

```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_end = False

class Trie:
    """Trie (Prefix Tree) implementation."""
    
    def __init__(self):
        self.root = TrieNode()
    
    def insert(self, word: str) -> None:
        """
        Insert word into trie.
        Time: O(m) where m is word length
        Space: O(m)
        """
        node = self.root
        
        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
        
        node.is_end = True
    
    def search(self, word: str) -> bool:
        """
        Check if word exists in trie.
        Time: O(m)
        Space: O(1)
        """
        node = self.root
        
        for char in word:
            if char not in node.children:
                return False
            node = node.children[char]
        
        return node.is_end
    
    def starts_with(self, prefix: str) -> bool:
        """
        Check if prefix exists.
        Time: O(m)
        Space: O(1)
        """
        node = self.root
        
        for char in prefix:
            if char not in node.children:
                return False
            node = node.children[char]
        
        return True
    
    def delete(self, word: str) -> bool:
        """
        Delete word from trie.
        Time: O(m)
        Space: O(m) for recursion
        """
        def _delete(node, word, index):
            if index == len(word):
                if not node.is_end:
                    return False
                node.is_end = False
                return len(node.children) == 0
            
            char = word[index]
            if char not in node.children:
                return False
            
            should_delete = _delete(node.children[char], word, index + 1)
            
            if should_delete:
                del node.children[char]
                return len(node.children) == 0 and not node.is_end
            
            return False
        
        return _delete(self.root, word, 0)

# Test
trie = Trie()
trie.insert("apple")
print(trie.search("apple"))      # True
print(trie.search("app"))        # False
print(trie.starts_with("app"))   # True
trie.insert("app")
print(trie.search("app"))        # True
```

### Trie with Array (for lowercase letters)

```python
class TrieNode:
    def __init__(self):
        self.children = [None] * 26  # a-z
        self.is_end = False

class Trie:
    def __init__(self):
        self.root = TrieNode()
    
    def _char_to_index(self, char):
        return ord(char) - ord('a')
    
    def insert(self, word: str) -> None:
        """Time: O(m)"""
        node = self.root
        
        for char in word:
            index = self._char_to_index(char)
            if not node.children[index]:
                node.children[index] = TrieNode()
            node = node.children[index]
        
        node.is_end = True
    
    def search(self, word: str) -> bool:
        """Time: O(m)"""
        node = self.root
        
        for char in word:
            index = self._char_to_index(char)
            if not node.children[index]:
                return False
            node = node.children[index]
        
        return node.is_end
```

### Trie with Word Count

```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_end = False
        self.count = 0  # Number of words ending here

class TrieWithCount:
    def __init__(self):
        self.root = TrieNode()
    
    def insert(self, word: str) -> None:
        """Time: O(m)"""
        node = self.root
        
        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
        
        node.is_end = True
        node.count += 1
    
    def count_words_equal_to(self, word: str) -> int:
        """Time: O(m)"""
        node = self.root
        
        for char in word:
            if char not in node.children:
                return 0
            node = node.children[char]
        
        return node.count if node.is_end else 0
    
    def count_words_starting_with(self, prefix: str) -> int:
        """Time: O(m)"""
        node = self.root
        
        for char in prefix:
            if char not in node.children:
                return 0
            node = node.children[char]
        
        # Count all words in this subtree
        def count_words(node):
            total = node.count if node.is_end else 0
            for child in node.children.values():
                total += count_words(child)
            return total
        
        return count_words(node)
```

---

## Common Operations

### Get All Words

```python
def get_all_words(self) -> list[str]:
    """
    Get all words in trie.
    Time: O(n) where n is total characters
    Space: O(n)
    """
    words = []
    
    def dfs(node, path):
        if node.is_end:
            words.append(''.join(path))
        
        for char, child in node.children.items():
            dfs(child, path + [char])
    
    dfs(self.root, [])
    return words
```

### Autocomplete

```python
def autocomplete(self, prefix: str, limit: int = 5) -> list[str]:
    """
    Get words with given prefix.
    Time: O(m + k) where k is results
    Space: O(k)
    """
    node = self.root
    
    # Navigate to prefix
    for char in prefix:
        if char not in node.children:
            return []
        node = node.children[char]
    
    # Find all words from this node
    results = []
    
    def dfs(node, path):
        if len(results) >= limit:
            return
        
        if node.is_end:
            results.append(prefix + ''.join(path))
        
        for char, child in node.children.items():
            dfs(child, path + [char])
    
    dfs(node, [])
    return results
```

### Longest Common Prefix

```python
def longest_common_prefix(self) -> str:
    """
    Find longest common prefix of all words.
    Time: O(m) where m is prefix length
    Space: O(1)
    """
    node = self.root
    prefix = []
    
    while len(node.children) == 1 and not node.is_end:
        char, child = next(iter(node.children.items()))
        prefix.append(char)
        node = child
    
    return ''.join(prefix)
```

---

## Common Patterns

### Pattern 1: Word Search in Matrix

```python
def find_words(board: list[list[str]], words: list[str]) -> list[str]:
    """
    Find words from dictionary in board (Word Search II).
    
    Time: O(m * n * 4^L) where L is max word length
    Space: O(k) where k is total characters in words
    """
    # Build trie
    trie = Trie()
    for word in words:
        trie.insert(word)
    
    rows, cols = len(board), len(board[0])
    result = set()
    
    def dfs(r, c, node, path):
        if node.is_end:
            result.add(path)
        
        if (r < 0 or r >= rows or c < 0 or c >= cols or
            board[r][c] not in node.children):
            return
        
        char = board[r][c]
        board[r][c] = '#'  # Mark visited
        
        for dr, dc in [(0,1), (1,0), (0,-1), (-1,0)]:
            dfs(r + dr, c + dc, node.children[char], path + char)
        
        board[r][c] = char  # Restore
    
    for r in range(rows):
        for c in range(cols):
            dfs(r, c, trie.root, "")
    
    return list(result)
```

### Pattern 2: Replace Words (Dictionary)

```python
def replace_words(dictionary: list[str], sentence: str) -> str:
    """
    Replace words with their shortest root.
    
    Time: O(d + s) where d is dict size, s is sentence length
    Space: O(d)
    """
    # Build trie with roots
    trie = Trie()
    for root in dictionary:
        trie.insert(root)
    
    def find_root(word):
        node = trie.root
        prefix = []
        
        for char in word:
            if char not in node.children:
                return word
            node = node.children[char]
            prefix.append(char)
            
            if node.is_end:
                return ''.join(prefix)
        
        return word
    
    words = sentence.split()
    return ' '.join(find_root(word) for word in words)
```

---

## Classic Problems

### 1. Implement Trie

```python
class Trie:
    def __init__(self):
        self.root = TrieNode()
    
    def insert(self, word: str) -> None:
        """Time: O(m)"""
        node = self.root
        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
        node.is_end = True
    
    def search(self, word: str) -> bool:
        """Time: O(m)"""
        node = self.root
        for char in word:
            if char not in node.children:
                return False
            node = node.children[char]
        return node.is_end
    
    def starts_with(self, prefix: str) -> bool:
        """Time: O(m)"""
        node = self.root
        for char in prefix:
            if char not in node.children:
                return False
            node = node.children[char]
        return True
```

### 2. Add and Search Word (with wildcards)

```python
class WordDictionary:
    """Support '.' wildcard."""
    
    def __init__(self):
        self.root = TrieNode()
    
    def add_word(self, word: str) -> None:
        """Time: O(m)"""
        node = self.root
        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
        node.is_end = True
    
    def search(self, word: str) -> bool:
        """
        Time: O(m) for exact, O(26^m) worst case with wildcards
        """
        def dfs(node, i):
            if i == len(word):
                return node.is_end
            
            char = word[i]
            
            if char == '.':
                # Try all children
                for child in node.children.values():
                    if dfs(child, i + 1):
                        return True
                return False
            else:
                if char not in node.children:
                    return False
                return dfs(node.children[char], i + 1)
        
        return dfs(self.root, 0)

# Test
wd = WordDictionary()
wd.add_word("bad")
wd.add_word("dad")
wd.add_word("mad")
print(wd.search("pad"))  # False
print(wd.search("bad"))  # True
print(wd.search(".ad"))  # True
print(wd.search("b.."))  # True
```

### 3. Word Search II

```python
def find_words(board: list[list[str]], words: list[str]) -> list[str]:
    """
    Time: O(m * n * 4^L)
    Space: O(k)
    """
    class TrieNode:
        def __init__(self):
            self.children = {}
            self.word = None
    
    # Build trie
    root = TrieNode()
    for word in words:
        node = root
        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
        node.word = word
    
    rows, cols = len(board), len(board[0])
    result = []
    
    def dfs(r, c, node):
        char = board[r][c]
        
        if char not in node.children:
            return
        
        next_node = node.children[char]
        
        if next_node.word:
            result.append(next_node.word)
            next_node.word = None  # Avoid duplicates
        
        board[r][c] = '#'  # Mark visited
        
        for dr, dc in [(0,1), (1,0), (0,-1), (-1,0)]:
            nr, nc = r + dr, c + dc
            if 0 <= nr < rows and 0 <= nc < cols and board[nr][nc] != '#':
                dfs(nr, nc, next_node)
        
        board[r][c] = char  # Restore
    
    for r in range(rows):
        for c in range(cols):
            dfs(r, c, root)
    
    return result
```

### 4. Longest Word in Dictionary

```python
def longest_word(words: list[str]) -> str:
    """
    Find longest word that can be built one char at a time.
    
    Time: O(n * m)
    Space: O(n * m)
    """
    trie = Trie()
    
    for word in words:
        trie.insert(word)
    
    longest = ""
    
    def dfs(node, path):
        nonlocal longest
        
        for char, child in node.children.items():
            if child.is_end:
                new_path = path + char
                if len(new_path) > len(longest) or \
                   (len(new_path) == len(longest) and new_path < longest):
                    longest = new_path
                dfs(child, new_path)
    
    dfs(trie.root, "")
    return longest
```

### 5. Replace Words

```python
def replace_words(dictionary: list[str], sentence: str) -> str:
    """
    Time: O(d + s)
    Space: O(d)
    """
    trie = Trie()
    for root in dictionary:
        trie.insert(root)
    
    def find_root(word):
        node = trie.root
        prefix = []
        
        for char in word:
            if char not in node.children:
                return word
            node = node.children[char]
            prefix.append(char)
            if node.is_end:
                return ''.join(prefix)
        
        return word
    
    words = sentence.split()
    return ' '.join(find_root(word) for word in words)

# Example:
# dictionary = ["cat", "bat", "rat"]
# sentence = "the cattle was rattled by the battery"
# Output: "the cat was rat by the bat"
```

### 6. Map Sum Pairs

```python
class MapSum:
    """Sum of values with given prefix."""
    
    def __init__(self):
        self.map = {}
        self.root = TrieNode()
    
    def insert(self, key: str, val: int) -> None:
        """Time: O(m)"""
        delta = val - self.map.get(key, 0)
        self.map[key] = val
        
        node = self.root
        for char in key:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
            node.sum_val = node.sum_val + delta if hasattr(node, 'sum_val') else delta
        
        node.is_end = True
    
    def sum(self, prefix: str) -> int:
        """Time: O(m)"""
        node = self.root
        
        for char in prefix:
            if char not in node.children:
                return 0
            node = node.children[char]
        
        return node.sum_val if hasattr(node, 'sum_val') else 0
```

### 7. Stream of Characters

```python
class StreamChecker:
    """Check if suffix of stream forms a word."""
    
    def __init__(self, words: list[str]):
        """Time: O(sum of word lengths)"""
        self.trie = TrieNode()
        self.stream = []
        self.max_len = 0
        
        # Insert reversed words
        for word in words:
            self.max_len = max(self.max_len, len(word))
            node = self.trie
            for char in reversed(word):
                if char not in node.children:
                    node.children[char] = TrieNode()
                node = node.children[char]
            node.is_end = True
    
    def query(self, letter: str) -> bool:
        """Time: O(m) where m is max word length"""
        self.stream.append(letter)
        
        # Keep only recent characters
        if len(self.stream) > self.max_len:
            self.stream.pop(0)
        
        # Search reversed stream
        node = self.trie
        for i in range(len(self.stream) - 1, -1, -1):
            char = self.stream[i]
            if char not in node.children:
                return False
            node = node.children[char]
            if node.is_end:
                return True
        
        return False
```

### 8. Lexicographical Numbers

```python
def lexical_order(n: int) -> list[int]:
    """
    Return 1 to n in lexicographical order using trie concept.
    
    Time: O(n)
    Space: O(1) excluding output
    """
    result = []
    current = 1
    
    for _ in range(n):
        result.append(current)
        
        if current * 10 <= n:
            current *= 10
        else:
            if current >= n:
                current //= 10
            current += 1
            
            while current % 10 == 0:
                current //= 10
    
    return result

# Example:
# n = 13
# Output: [1,10,11,12,13,2,3,4,5,6,7,8,9]
```

### 9. Maximum XOR of Two Numbers

```python
def find_maximum_xor(nums: list[int]) -> int:
    """
    Use trie with binary representation.
    
    Time: O(n * 32)
    Space: O(n * 32)
    """
    class TrieNode:
        def __init__(self):
            self.children = {}
    
    root = TrieNode()
    
    # Insert all numbers as 32-bit binary
    for num in nums:
        node = root
        for i in range(31, -1, -1):
            bit = (num >> i) & 1
            if bit not in node.children:
                node.children[bit] = TrieNode()
            node = node.children[bit]
    
    max_xor = 0
    
    # For each number, find maximum XOR
    for num in nums:
        node = root
        current_xor = 0
        
        for i in range(31, -1, -1):
            bit = (num >> i) & 1
            toggled = 1 - bit
            
            # Try opposite bit for max XOR
            if toggled in node.children:
                current_xor |= (1 << i)
                node = node.children[toggled]
            else:
                node = node.children[bit]
        
        max_xor = max(max_xor, current_xor)
    
    return max_xor
```

### 10. Palindrome Pairs

```python
def palindrome_pairs(words: list[str]) -> list[list[int]]:
    """
    Find pairs where concatenation forms palindrome.
    
    Time: O(n * m²) where n is word count, m is max length
    Space: O(n * m)
    """
    def is_palindrome(s):
        return s == s[::-1]
    
    word_dict = {word: i for i, word in enumerate(words)}
    result = []
    
    for i, word in enumerate(words):
        for j in range(len(word) + 1):
            prefix = word[:j]
            suffix = word[j:]
            
            # Check if prefix reversed exists
            if is_palindrome(suffix):
                target = prefix[::-1]
                if target in word_dict and word_dict[target] != i:
                    result.append([i, word_dict[target]])
            
            # Check if suffix reversed exists
            if j != len(word) and is_palindrome(prefix):
                target = suffix[::-1]
                if target in word_dict and word_dict[target] != i:
                    result.append([word_dict[target], i])
    
    return result
```

---

## Interview Tips

### 1. When to Use Trie

```python
# Use trie for:
- Prefix matching
- Autocomplete
- Spell checking
- Word games
- IP routing
- String search in dictionary
```

### 2. Trie vs Hash Map

```python
# Hash Map:
- Exact word lookup: O(m)
- Prefix search: O(n * m) - check all words
- Space: O(n * m)

# Trie:
- Exact word lookup: O(m)
- Prefix search: O(m) - navigate to prefix
- Autocomplete: O(m + k)
- Space: O(alphabet_size * n * m) worst case
```

### 3. Common Trie Patterns

```python
# 1. Basic structure
class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_end = False

# 2. Navigation
node = root
for char in word:
    if char not in node.children:
        return False  # or create
    node = node.children[char]

# 3. DFS for all words
def dfs(node, path):
    if node.is_end:
        result.append(''.join(path))
    for char, child in node.children.items():
        dfs(child, path + [char])
```

### 4. Optimizations

```python
# Use array for fixed alphabet
class TrieNode:
    def __init__(self):
        self.children = [None] * 26  # a-z only
        self.is_end = False

# Store additional info in nodes
class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_end = False
        self.count = 0      # Frequency
        self.word = None    # Store full word
```

---

## Practice Problems

### Easy
1. [Implement Trie](https://leetcode.com/problems/implement-trie-prefix-tree/)
2. [Longest Word in Dictionary](https://leetcode.com/problems/longest-word-in-dictionary/)

### Medium
1. [Add and Search Word](https://leetcode.com/problems/design-add-and-search-words-data-structure/)
2. [Replace Words](https://leetcode.com/problems/replace-words/)
3. [Map Sum Pairs](https://leetcode.com/problems/map-sum-pairs/)
4. [Lexicographical Numbers](https://leetcode.com/problems/lexicographical-numbers/)
5. [Word Search II](https://leetcode.com/problems/word-search-ii/)
6. [Implement Magic Dictionary](https://leetcode.com/problems/implement-magic-dictionary/)

### Hard
1. [Stream of Characters](https://leetcode.com/problems/stream-of-characters/)
2. [Maximum XOR of Two Numbers](https://leetcode.com/problems/maximum-xor-of-two-numbers-in-an-array/)
3. [Palindrome Pairs](https://leetcode.com/problems/palindrome-pairs/)
4. [Word Squares](https://leetcode.com/problems/word-squares/)

---

## Summary

### Key Takeaways
- ✅ Trie = Tree for string storage
- ✅ O(m) operations (m = word length)
- ✅ Perfect for prefix operations
- ✅ Shares common prefixes
- ✅ Autocomplete and spell check

### Common Patterns
```python
# 1. Basic trie operations
class Trie:
    def insert(self, word):
        node = self.root
        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
        node.is_end = True
    
    def search(self, word):
        node = self.root
        for char in word:
            if char not in node.children:
                return False
            node = node.children[char]
        return node.is_end

# 2. DFS for collecting words
def dfs(node, path, results):
    if node.is_end:
        results.append(''.join(path))
    for char, child in node.children.items():
        dfs(child, path + [char], results)

# 3. Wildcard search
def search_with_wildcard(node, word, i):
    if i == len(word):
        return node.is_end
    
    if word[i] == '.':
        return any(search_with_wildcard(child, word, i+1)
                   for child in node.children.values())
    
    if word[i] not in node.children:
        return False
    return search_with_wildcard(node.children[word[i]], word, i+1)
```

---

**Next**: [Graphs →](../11-graphs/README.md)

**Happy Coding! 🚀**
