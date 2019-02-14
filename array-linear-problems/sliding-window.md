---
description: 'Dec 18, Feb 19'
---

# Sliding window

#### 3. Longest Substring Without Repeating Characters

```python
def lengthOfLongestSubstring(s):
    result = 0
    start = 0
    nextStart = {}
    
    for i, char in enumerate(s):
        # tricky here, should make sure the pointer will not go backwards
        start = max(start, nextStart.get(char, start))
        result = max(result, i - start + 1)
        nextStart[char] = i + 1
    return result
```



### 904. Fruit Into Baskets

1. invariant design  
2. put more effect in **test** design

```python
def totalFruit(self, tree):
    result = 0
    # problem of choice: choose the start, brute force: O(n^2), n starts, O(n) to validate
    # sliding window to optimize
    basket = [] # int[]
    count = [] # int[], parallel to basket, keep track of the count
    start = 0
    for end, fruit in enumerate(tree):
        if fruit in basket: # bug: no check for accumulate
            count[basket.index(fruit)] += 1
        else:
            while len(basket) > 1:
                expiredFruit = tree[start]
                index = basket.index(expiredFruit)
                count[index] -= 1
                if count[index] == 0: # pop that fruit from the basket
                    del basket[index]
                    del count[index]
                start += 1
            # invariant before adding a new type of fruit in the basket:
            assert fruit not in basket and len(basket) < 2
            basket.append(fruit)
            count.append(1)
        result = max(result, end - start + 1)
    return result
    
# tests
# [1]
# [1,2]
# [1,2,1]
# [1,2,3] - for expiration
# [1,1,2,2,3]
# [1,3,2,3,2,1,1,1,1]
```

### 76. Minimum Window Substring

```python
def minWindow(self, s, t):
    # for each ending position, possibly an ending for the shortest good substring
    # when an end fixed, before counting the length, advance the start pointer as far as possible
    
    # bug: wrong init value
    i, j = 0, len(s) - 1 # start and end indices for the result substring
    # maintain a map for char count: val <= 0 means that char satisfied
    char2count = Counter(t)
    start = 0
    unsatisfied = len(t) # the value never increase
    for end, char in enumerate(s):
        if char2count[char] > 0: # bug: decrease when it is positive
            unsatisfied -= 1
        char2count[char] -= 1
        if unsatisfied == 0:
            # invariant: start as far as possible such that 
            # we don't break the requirement
            while char2count[s[start]] < 0:
                char2count[s[start]] += 1
                start += 1
            if end - start < j - i: # bug: didn't compare
                i, j = start, end
    return s[i:j+1] if unsatisfied == 0 else ""

# tests
# "ba", "a"
# "abc", "ab"
# "abc", "bc"
# "abc", "cb"
# "abc", "ac"
# "aba", "aa" - duplicate
# "bcuiopcbac", "bcc" -> "cbac" - overidden result
# "abc", "cd" - not satisfied!!!!
```

Feb 19

```python
from collections import Counter
def minWindow(self, s, t):
    # maintain a window that containing all characters in T, but redundant chars on the left reduced
    
    char2count = Counter(t)
    unsat = len(char2count)  # unsatisfied char
    rge = None  # [int, int]?
    
    i = 0  # the left pointer
    for j, char in enumerate(s):
        # include the char the right pointer is pointing to
        if char2count[char] == 1:
            unsat -= 1
        char2count[char] -= 1
        # advance the left pointer
        while i < j and char2count[s[i]] < 0:  # bug: s[i] -> char
            char2count[s[i]] += 1
            i += 1
        if unsat:
            continue
        if not rge or j - i < rge[1] - rge[0]:
            rge = [i, j]
    return s[rge[0] : rge[1]+1] if rge else ""
# tests
# "", "A"
```

