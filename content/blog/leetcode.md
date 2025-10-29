---
title: 'LeetCode Guide'
date: 2025-10-29
draft: false
description: 'A comprehensive guide to mastering technical interviews through deliberate practice'
slug: 'leetcode-guide'
tags: ['Algorithms', 'Data Structures', 'Interview Prep', 'LeetCode']
---

Let's address the elephant in the room: LeetCode isn't about becoming a better programmer. It's about passing technical interviews. The skills overlap, but they're not identical. Accepting this makes the process less frustrating.

## The Pattern Recognition Game

Technical interviews aren't testing if you can invent algorithms—they're testing if you can recognize and apply patterns. Here are the patterns that cover 90% of problems:

### 1. Two Pointers

**When to use**: Arrays, strings, linked lists with comparison or search operations

**Classic Problems**:

- Two Sum (sorted array)
- Container With Most Water
- Trapping Rain Water
- Remove Duplicates

**The Template**:

```python
def two_pointer_template(arr):
    left, right = 0, len(arr) - 1

    while left < right:
        # Process current state
        if condition_met:
            return result
        elif need_smaller:
            left += 1
        else:
            right -= 1

    return not_found
```

**Key Insight**: Often converts O(n²) brute force to O(n) or O(n log n) with sorting.

### 2. Sliding Window

**When to use**: Subarray/substring problems with contiguous elements

**Fixed Window**:

```python
def fixed_window(arr, k):
    window_sum = sum(arr[:k])
    max_sum = window_sum

    for i in range(k, len(arr)):
        window_sum = window_sum - arr[i-k] + arr[i]
        max_sum = max(max_sum, window_sum)

    return max_sum
```

**Variable Window**:

```python
def variable_window(s, condition):
    left = 0
    result = 0
    window = {}  # or whatever you're tracking

    for right in range(len(s)):
        # Expand window
        add_to_window(s[right])

        # Contract window while invalid
        while not valid(window):
            remove_from_window(s[left])
            left += 1

        # Update result
        result = max(result, right - left + 1)

    return result
```

### 3. Fast & Slow Pointers

**When to use**: Cycle detection, middle element, linked list problems

**Classic Applications**:

```python
def has_cycle(head):
    slow = fast = head

    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        if slow == fast:
            return True

    return False

def find_middle(head):
    slow = fast = head

    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next

    return slow
```

### 4. Tree Traversal Patterns

**The Three Questions for Any Tree Problem**:

1. Can I solve it by visiting nodes in a specific order? (Traversal)
2. Can I solve it by getting info from children first? (Post-order)
3. Can I solve it top-down passing info to children? (Pre-order)

**The Universal Template**:

```python
def tree_recursion(root, additional_params):
    # Base case
    if not root:
        return base_value

    # Pre-order processing

    # Recurse
    left_result = tree_recursion(root.left, updated_params)
    right_result = tree_recursion(root.right, updated_params)

    # Post-order processing

    return combined_result
```

### 5. BFS / Level-Order

**When to use**: Shortest path, level-by-level processing, trees/graphs

**The Template That Handles Everything**:

```python
from collections import deque

def bfs_template(start):
    queue = deque([start])
    visited = {start}
    level = 0

    while queue:
        # Process entire level at once
        level_size = len(queue)
        for _ in range(level_size):
            node = queue.popleft()

            # Process node
            if is_target(node):
                return level

            # Add neighbors
            for neighbor in get_neighbors(node):
                if neighbor not in visited:
                    visited.add(neighbor)
                    queue.append(neighbor)

        level += 1

    return -1
```

### 6. DFS / Backtracking

**When to use**: All paths, all combinations, all permutations, puzzles

**The Backtracking Template**:

```python
def backtrack(choices, path, result):
    # Base case
    if is_complete(path):
        result.append(path[:])  # Important: copy the path
        return

    for i, choice in enumerate(choices):
        # Skip invalid choices
        if not is_valid(choice, path):
            continue

        # Make choice
        path.append(choice)

        # Recurse with remaining choices
        backtrack(choices[i+1:], path, result)  # or choices[:i] + choices[i+1:] for perms

        # Undo choice
        path.pop()
```

### 7. Dynamic Programming

**The Two Questions**:

1. Can I break this into overlapping subproblems?
2. Can I define the solution recursively?

**Top-Down (Memoization)**:

```python
def dp_recursive(n, memo={}):
    # Base case
    if n <= 1:
        return base_value

    # Check memo
    if n in memo:
        return memo[n]

    # Recurrence relation
    memo[n] = dp_recursive(n-1) + dp_recursive(n-2)  # or whatever

    return memo[n]
```

**Bottom-Up (Tabulation)**:

```python
def dp_iterative(n):
    if n <= 1:
        return base_value

    dp = [0] * (n + 1)
    dp[0] = base1
    dp[1] = base2

    for i in range(2, n + 1):
        dp[i] = dp[i-1] + dp[i-2]  # recurrence relation

    return dp[n]
```

**Space Optimization** (when you only need previous few values):

```python
def dp_optimized(n):
    if n <= 1:
        return base_value

    prev2, prev1 = base1, base2

    for i in range(2, n + 1):
        current = prev1 + prev2
        prev2, prev1 = prev1, current

    return prev1
```

### 8. Binary Search

**Beyond Sorted Arrays**:

```python
def binary_search_template(search_space):
    left, right = min_value, max_value

    while left < right:
        mid = left + (right - left) // 2

        if condition(mid):
            right = mid  # or left = mid + 1
        else:
            left = mid + 1  # or right = mid

    return left
```

**The Trick**: If you can frame the problem as "find the minimum/maximum value where condition is true," use binary search.

## Data Structure Instant Decisions

### When to Use What

**Array/List**:

- Need index access
- Fixed size or know size upfront
- Cache-friendly operations

**HashMap/Dict**:

- O(1) lookup needed
- Counting occurrences
- Mapping relationships
- Finding pairs/complements

**Set**:

- Uniqueness matters
- O(1) membership testing
- Finding duplicates

**Stack**:

- Matching brackets/parentheses
- Nearest greater/smaller element
- Evaluate expressions
- Backtracking (implicit with recursion)

**Queue/Deque**:

- BFS
- Level-order traversal
- Sliding window maximum
- Process in order

**Heap/Priority Queue**:

- K-th largest/smallest
- Top K frequent
- Merge K sorted lists
- Dijkstra's algorithm

**Trie**:

- Prefix matching
- Autocomplete
- Word search in grid

## The Time Complexity Reality Check

Stop memorizing—understand the patterns:

**O(1)**: Direct access, math operations
**O(log n)**: Binary search, balanced tree operations
**O(n)**: Single pass through data
**O(n log n)**: Sorting, divide & conquer
**O(n²)**: Nested loops, comparing all pairs
**O(2ⁿ)**: Subsets, recursive branching
**O(n!)**: Permutations

**The Interview Optimization Path**:

1. Brute force (usually O(n²) or worse)
2. Can I sort first? (O(n log n))
3. Can I use a hash map? (O(n) space for O(1) lookup)
4. Can I use two pointers? (O(1) space)
5. Is there a greedy approach?
6. Do I need DP?

## Common Gotchas and How to Avoid Them

### Python Specific Traps

```python
# Gotcha 1: Mutable default arguments
def bad(arr=[]):  # DON'T
    arr.append(1)
    return arr

def good(arr=None):  # DO
    if arr is None:
        arr = []
    arr.append(1)
    return arr

# Gotcha 2: Integer division
result = a // b  # Floor division (what you usually want)
result = int(a / b)  # Truncation toward zero (different for negatives!)

# Gotcha 3: List reference vs copy
path.append(current_path)  # BAD - stores reference
path.append(current_path[:])  # GOOD - stores copy

# Gotcha 4: String immutability
s[0] = 'a'  # ERROR - strings are immutable
s = list(s)  # Convert to list first
s[0] = 'a'
s = ''.join(s)
```

### The Edge Cases Checklist

Always test:

- Empty input ([], "", None)
- Single element
- All same elements
- Negative numbers
- Overflow (for int problems)
- Cycles (for graphs/linked lists)
- Disconnected components (for graphs)

## The Interview Strategy

### Before You Code

1. **Clarify constraints**: Input size? Value ranges? Can I modify input?
2. **Work through examples**: Start small, find patterns
3. **Discuss approach**: Brute force → optimized
4. **Analyze complexity**: Time and space
5. **Then code**: Only after agreement on approach

### While Coding

- **Think out loud**: Silence is your enemy
- **Write clean code**: Variable names matter
- **Handle edges**: But don't obsess initially
- **Test as you go**: Don't wait until the end

### The 45-Minute Timeline

- 0-5 min: Understand problem
- 5-10 min: Work through examples
- 10-15 min: Discuss approach
- 15-35 min: Code solution
- 35-40 min: Test and debug
- 40-45 min: Discuss optimizations

## The Practice Strategy That Works

### Week 1-2: Fundamentals

- 5 array problems
- 5 string problems
- 5 linked list problems
- Focus on brute force, don't optimize yet

### Week 3-4: Core Patterns

- 10 two-pointer problems
- 10 sliding window problems
- Learn the templates, apply mechanically

### Week 5-6: Trees and Graphs

- 10 tree problems
- 10 graph problems (BFS/DFS)
- Master the traversal templates

### Week 7-8: Dynamic Programming

- Start with classic problems (Fibonacci, coin change)
- Focus on identifying recurrence relations
- Don't memorize solutions

### Week 9-12: Mixed Practice

- 3-5 problems daily
- Time yourself
- Focus on mediums
- One hard per week

### The Spaced Repetition Secret

- Solve problem
- Try again in 3 days
- Try again in 1 week
- Try again in 1 month

If you can't solve it from scratch each time, you haven't learned the pattern.

## When You're Stuck

### The Unstuck Protocol

1. **Re-read the problem**: Did I miss constraints?
2. **Try more examples**: Look for patterns
3. **Simplify**: Solve for n=2, then generalize
4. **Consider all patterns**: Go through the list above
5. **Check discussions after 30 minutes**: Learn, don't just copy

### The Learning Mindset

- **It's not about intelligence**: It's about pattern recognition
- **Everyone struggles initially**: Including FAANG engineers
- **Progress isn't linear**: Breakthrough moments happen suddenly
- **Quality over quantity**: Understanding 100 problems beats rushing through 500

## The Meta-Game

### What Interviewers Actually Care About

1. **Problem-solving approach**: Not just the solution
2. **Communication**: Can you explain your thinking?
3. **Code quality**: Is it readable and maintainable?
4. **Debugging skills**: How do you handle bugs?
5. **Optimization**: Can you improve your solution?

### Red Flags to Avoid

- Jumping to code immediately
- Not asking clarifying questions
- Writing unreadable code
- Giving up too quickly
- Not testing your solution
- Being arrogant or dismissive

## Conclusion: The Long Game

LeetCode is a game with specific rules. Learn the rules, practice the patterns, and you'll improve. But remember:

1. **It's a learnable skill**: Not a measure of your worth as a developer
2. **Consistency beats intensity**: 30 minutes daily beats 5-hour weekend sessions
3. **Understanding beats memorization**: Learn patterns, not solutions
4. **It's temporary**: Once you get the job, you'll rarely use these specific skills

The developers who succeed at LeetCode aren't necessarily the smartest—they're the ones who figured out it's just pattern matching with extra steps.
