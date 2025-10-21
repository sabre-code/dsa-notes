# 🚀 Getting Started

Welcome to your Python DSA journey! This guide will help you navigate the repository and start learning effectively.

---

## 📂 Repository Structure

```
documentation/
├── README.md                          # Main overview
├── PROJECT_STATUS.md                  # What's done, what's next
├── CHEAT_SHEET.md                     # Quick reference
├── GETTING_STARTED.md                 # You are here!
│
├── 01-python-fundamentals/            # Python basics
│   └── README.md                      # Complete Python guide
│
├── 02-complexity-analysis/            # Time & Space complexity
│   └── README.md                      # Big O, analysis techniques
│
├── 03-data-structures/                # Core data structures
│   ├── 01-arrays-lists/
│   │   └── README.md                  # Arrays with patterns & problems
│   ├── 02-strings/                    # Coming soon
│   ├── 03-linked-lists/               # Coming soon
│   └── ...
│
├── 04-algorithm-patterns/             # Problem-solving patterns
│   ├── 01-two-pointers/
│   │   └── README.md                  # Complete two pointers guide
│   ├── 02-sliding-window/             # Coming soon
│   └── ...
│
├── 05-problem-solving/                # Strategies & techniques
│   └── README.md                      # Coming soon
│
├── 06-interview-questions/            # Famous problem sets
│   ├── blind-75/
│   │   └── README.md                  # Blind 75 with solutions
│   ├── grind-75/                      # Coming soon
│   └── ...
│
├── 07-study-plans/                    # Learning roadmaps
│   └── README.md                      # 4/8/12 week plans
│
└── 08-resources/                      # Additional materials
    └── README.md                      # Coming soon
```

---

## 🎯 Choose Your Path

### Path 1: Complete Beginner (12 weeks)

**Start here if**: You're new to programming or Python

**Steps**:
1. 📖 Read [Python Fundamentals](./01-python-fundamentals/README.md) (Week 1)
2. 📖 Study [Complexity Analysis](./02-complexity-analysis/README.md) (Week 2)
3. 📚 Follow [12-Week Deep Dive Plan](./07-study-plans/README.md#-12-week-deep-dive)
4. 💪 Practice 1-2 problems daily
5. 🔄 Review weekly

**Resources you'll use**:
- This repository (theory + examples)
- [LeetCode](https://leetcode.com) (practice)
- [Python.org](https://python.org) (official docs)

---

### Path 2: Interview Prep (4-8 weeks)

**Start here if**: You know Python basics, need to prepare for interviews

**Steps**:
1. ✅ Quick review: [Python Fundamentals](./01-python-fundamentals/README.md)
2. ✅ Refresh: [Complexity Analysis](./02-complexity-analysis/README.md)
3. 🎯 Choose plan:
   - Urgent (4 weeks): [Crash Course](./07-study-plans/README.md#-4-week-crash-course)
   - Comfortable (8 weeks): [Comprehensive Plan](./07-study-plans/README.md#-8-week-comprehensive-plan)
4. 🏆 Focus on [Blind 75](./06-interview-questions/blind-75/README.md)
5. 💻 Practice mock interviews

**Daily schedule** (2-3 hours):
- 30 min: Review theory/patterns
- 90 min: Solve 2-3 problems
- 30 min: Review solutions

---

### Path 3: Topic-Specific Learning

**Start here if**: You need to master specific topics

**Pick your weak areas**:
- 📊 [Arrays & Lists](./03-data-structures/01-arrays-lists/README.md)
- 🔤 Strings (coming soon)
- 🔗 Linked Lists (coming soon)
- 🌳 Trees (coming soon)
- 📈 Graphs (coming soon)
- 💎 Dynamic Programming (coming soon)

**For each topic**:
1. Read theory section
2. Understand patterns
3. Solve 5-10 easy problems
4. Solve 5-10 medium problems
5. Attempt 2-3 hard problems

---

## 📚 First Week Action Plan

### Day 1-2: Python Setup & Basics
1. Install Python 3.8+ ([python.org](https://python.org))
2. Read [Python Fundamentals](./01-python-fundamentals/README.md)
3. Practice basic syntax in Python REPL
4. Create a LeetCode account

### Day 3: Complexity Analysis
1. Read [Complexity Analysis](./02-complexity-analysis/README.md)
2. Practice calculating complexity of simple functions
3. Understand Big O notation

### Day 4-5: Arrays & Two Pointers
1. Study [Arrays & Lists](./03-data-structures/01-arrays-lists/README.md)
2. Read [Two Pointers Pattern](./04-algorithm-patterns/01-two-pointers/README.md)
3. Solve these problems:
   - Two Sum
   - Valid Palindrome
   - Move Zeroes

### Day 6: Practice Day
1. Solve 3-5 easy array problems
2. Review solutions
3. Note down patterns

### Day 7: Review & Plan
1. Review everything learned
2. Choose your study plan
3. Set weekly goals

---

## 🛠️ Setup Your Environment

### 1. Install Python

```bash
# Check if Python is installed
python --version  # or python3 --version

# Should be Python 3.8 or higher
```

**Download**: [python.org/downloads](https://python.org/downloads)

### 2. Code Editor

Choose one:
- **VS Code** (recommended): [code.visualstudio.com](https://code.visualstudio.com)
- **PyCharm**: [jetbrains.com/pycharm](https://jetbrains.com/pycharm)
- **Sublime Text**: [sublimetext.com](https://sublimetext.com)

### 3. Practice Platform

Create accounts on:
- **LeetCode** (primary): [leetcode.com](https://leetcode.com)
- **HackerRank**: [hackerrank.com](https://hackerrank.com)
- **CodeForces**: [codeforces.com](https://codeforces.com)

### 4. Optional Tools

```bash
# Testing framework
pip install pytest

# Code formatting
pip install black

# Linting
pip install pylint
```

---

## 📝 How to Use This Repository

### For Learning
1. **Read sequentially**: Start from fundamentals
2. **Run code examples**: Don't just read, type and execute
3. **Solve problems**: Practice after each section
4. **Take notes**: Write your own summaries

### For Reference
1. **Use search**: Ctrl+F to find specific topics
2. **Bookmark sections**: Keep important pages handy
3. **Use cheat sheet**: [CHEAT_SHEET.md](./CHEAT_SHEET.md) for quick lookup

### For Interview Prep
1. **Follow study plans**: Structured learning path
2. **Track progress**: Check off completed problems
3. **Review regularly**: Space out your practice
4. **Mock interviews**: Practice under time pressure

---

## 💡 Learning Tips

### 1. Understand, Don't Memorize
```python
# ❌ Bad: Memorize this code
def two_sum(nums, target):
    seen = {}
    for i, num in enumerate(nums):
        if target - num in seen:
            return [seen[target-num], i]
        seen[num] = i

# ✅ Good: Understand the pattern
# "Use hash map to store complements for O(1) lookup"
```

### 2. Practice Actively
- Don't just read solutions
- Try solving first (even if wrong)
- Learn from mistakes

### 3. Focus on Patterns
- Recognize when to use two pointers
- Identify sliding window problems
- Spot DP opportunities

### 4. Review Regularly
- Day 1: Solve problem
- Day 3: Solve again
- Week 1: Review pattern
- Week 2: Solve variations

### 5. Track Your Progress
Create a spreadsheet:
```
| Date | Problem | Difficulty | Solved? | Time | Pattern | Notes |
|------|---------|------------|---------|------|---------|-------|
```

---

## 🎯 Setting Goals

### Daily Goals
- [ ] Spend 1-2 hours on DSA
- [ ] Solve 1-2 problems
- [ ] Review 1 concept

### Weekly Goals
- [ ] Complete 1 data structure
- [ ] Master 1 pattern
- [ ] Solve 10-15 problems
- [ ] Review previous week

### Monthly Goals
- [ ] Complete 4 data structures
- [ ] Master 4 patterns
- [ ] Solve 50+ problems
- [ ] Attempt 1 mock interview

---

## ❓ Common Questions

### Q: I'm stuck on a problem. What should I do?
1. Read problem carefully
2. Try 20-30 minutes on your own
3. Look at hints (not solution)
4. If still stuck, read solution and understand deeply
5. Solve again tomorrow

### Q: How many problems should I solve?
- **Minimum**: 100 problems (Blind 75 + variations)
- **Good**: 200 problems
- **Excellent**: 300+ problems

Quality > Quantity. Better to solve 50 problems deeply than 200 superficially.

### Q: How long will it take?
- **Crash course**: 4 weeks (3-4 hours/day)
- **Balanced**: 8 weeks (2-3 hours/day)
- **Thorough**: 12 weeks (1-2 hours/day)

### Q: What if I don't understand something?
1. Read again slowly
2. Try examples on paper
3. Google the concept
4. Watch YouTube tutorials
5. Ask in forums (Stack Overflow, Reddit)

### Q: Should I learn all data structures?
**Must-know**:
- Arrays, Strings, Hash Tables
- Linked Lists, Stacks, Queues
- Trees (Binary, BST)
- Graphs
- Heaps

**Good to know**:
- Tries
- Union-Find
- Segment Trees (for advanced roles)

---

## 📚 Recommended Resources

### Websites
- [LeetCode](https://leetcode.com) - Primary practice
- [NeetCode](https://neetcode.io) - Video explanations
- [GeeksforGeeks](https://geeksforgeeks.org) - Tutorials

### YouTube Channels
- [NeetCode](https://youtube.com/@NeetCode)
- [Abdul Bari](https://youtube.com/@abdul_bari)
- [Back To Back SWE](https://youtube.com/@BackToBackSWE)

### Books
- "Cracking the Coding Interview" - Gayle McDowell
- "Elements of Programming Interviews in Python"
- "Introduction to Algorithms" (CLRS) - Advanced

---

## 🎮 Your First Challenge

Ready to start? Solve these 3 problems today:

1. **Two Sum** (Easy)
   - [LeetCode #1](https://leetcode.com/problems/two-sum/)
   - Pattern: Hash Map
   - Time: 15 minutes

2. **Valid Palindrome** (Easy)
   - [LeetCode #125](https://leetcode.com/problems/valid-palindrome/)
   - Pattern: Two Pointers
   - Time: 15 minutes

3. **Best Time to Buy and Sell Stock** (Easy)
   - [LeetCode #121](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)
   - Pattern: Single Pass
   - Time: 15 minutes

**After solving**:
- ✅ Check solutions in [Blind 75](./06-interview-questions/blind-75/README.md)
- ✅ Understand the patterns
- ✅ Note down learnings

---

## 📅 Sample Study Schedule

### Weekday (2 hours)
```
Evening:
6:00 PM - Review theory (30 min)
6:30 PM - Solve problems (90 min)
```

### Weekend (4 hours)
```
Morning:
9:00 AM  - Learn new topic (60 min)
10:00 AM - Practice problems (90 min)
11:30 AM - Break

Afternoon:
2:00 PM - Review & revise (60 min)
3:00 PM - Mock interview (30 min)
```

---

## 🎯 Next Steps

1. ✅ Read [Python Fundamentals](./01-python-fundamentals/README.md)
2. ✅ Study [Complexity Analysis](./02-complexity-analysis/README.md)
3. ✅ Pick a [Study Plan](./07-study-plans/README.md)
4. 💪 Start solving [Blind 75](./06-interview-questions/blind-75/README.md)
5. 📊 Track progress in [PROJECT_STATUS.md](./PROJECT_STATUS.md)

---

## 💬 Final Words

> "The expert in anything was once a beginner."

**Remember**:
- Everyone struggles initially
- Consistency > Intensity
- Focus on understanding > speed
- Mistakes are learning opportunities
- Progress is not always linear

**You've got this! 🚀**

Start with [Python Fundamentals](./01-python-fundamentals/README.md) →

---

**Questions?** Check [PROJECT_STATUS.md](./PROJECT_STATUS.md) for repository updates!

**Need quick reference?** See [CHEAT_SHEET.md](./CHEAT_SHEET.md)

**Happy Coding! 🎉**
