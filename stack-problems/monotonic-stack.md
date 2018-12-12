---
description: optimize quadratic to linear
---

# Monotonic stack

## Recipes

* FILO pattern
* Identify what to find, see 739, 84
* determine monotonicity -&gt; inc, non-dec; dec, non-inc

most tricky problem: 132 pattern

### How do we develop into this method?

brute force: **from perspective of each point**, see analysis for problem 42

## Problems

### 739. Daily Temperatures

{% hint style="info" %}
**what**: for index i, find the leftmost index j: `j > i and T[j] > T[i]`; scanning from the back

**mono**: strictly decreasing, while we find`T[k] >= T[stack[-1]]`, index stack\[-1\] is no longer needed
{% endhint %}

tool for optimizing this kind of problems

### 84. Largest Rectangle in Histogram

{% hint style="info" %}
**what**: for index `t`  
the leftmost i: `i <= t and heights[k] >= heights[t] for k in [i, t]`  
the rightmost j: `j >= t and heights[k] >= heights[t] for k in [t, j]`

**mono**: strictly increasing, H\[k\] &lt;= H\[stack\[-1\]\] , we can skip the H\[stack\[-1\]\] which is larger or equal

!!! **121 pattern**
{% endhint %}

```python
def largestRectangleArea(self, heights):
    n = len(heights)
    leftmost = [-1] * n
    rightmost = [-1] * n

    stack = []
    for i, h in enumerate(heights): # from start 
        while stack and h <= heights[stack[-1]]:
            stack.pop()
        leftmost[i] = stack and (stack[-1] + 1) or 0
        stack.append(i)

    stack = []
    for j in range(n-1, -1, -1): # from back
        while stack and heights[j] <= heights[stack[-1]]:
            stack.pop()
        rightmost[j] = stack[-1] - 1 if stack else n-1 # bug: stack and (stack[-1] - 1) or n-1, stack[-1] - 1 can be zero!!!
        stack.append(j)
    
    result = 0
    for k, (i, j) in enumerate(zip(leftmost, rightmost)):
        height = heights[k]
        result = max(result, height * (j - i + 1))
    return result

# tests
# [1,3,1]
# [1,1,1,3]
# [1,1,5,3,3,2]

def largestRectangleArea(self, heights):
    # a trick here:
    heights.append(0) # so that all bars that have height > 0 are considered, eliminate the post-processing
    
    # maintain an increasing stack: 121 pattern
    stack = [] # indices
    
    result = 0
    for right, rightHeight in enumerate(heights):
        # maintain the monotonicity: increasing
        while stack and rightHeight <= heights[stack[-1]]:
            index = stack.pop()
            width = right - (stack[-1] if stack else -1) - 1
            height = heights[index]
            result = max(result, height * width)
        stack.append(right)
        
    # corner: [1,2,3]
    # increasing stack remained
    # n = len(heights)
    # while stack:
    #     index = stack.pop()
    #     width = n - 1 - (stack[-1] if stack else -1) # the top has no bar on the right that is greater than it
    #     height = heights[index]
    #     result = max(result, height * width)
    return result

# tests
# [1,2,3]
```

### 42. Trapping Rain Water

what: find **212 pattern**: `i < k < j, height[i] > height[k] and height[k] < height[j]`

mono: a little bit tricky here, consider case \[3,2,1,2,5\]

{% hint style="info" %}
brute force: from perspective of each bar k:  
find the right most i: `i > k and height[i] >= height[k]`  
find the left most j: `j > k and height[j] >= height[k]`

**`212 pattern`**
{% endhint %}

```python
def trap(self, height):
    # tag: stack, maintain a decreasing stack
    stack = [] # store the indexes of the left end
    
    result = 0
    for rightIndex, rightHeight in enumerate(height):
        
        while stack and rightHeight >= height[stack[-1]]:
            base = height[stack.pop()]
            if stack:
                leftIndex = stack[-1]
                leftHeight = height[leftIndex]
                # accumulate when `1` in `212` pattern poped
                result += (min(leftHeight, rightHeight) - base) * (rightIndex - leftIndex - 1)
        # bug: [4,2,3]
        stack.append(rightIndex)
    return result
```

buggy code, order of doing things

4 different approaches to tackle this problem

### 456. 132 Pattern

![](../.gitbook/assets/456.jpeg)

```python
def find132pattern(self, nums):
    # idea: decreasing stack + min cache, increasing stack doesn't seem to work
    mn = float('inf')
    stack = [] # (int, int)[], element and the min val before it
    
    for i, num in enumerate(nums):
        if mn < num:   
            while stack and num >= stack[-1][0]: # maintain decreasing 
                stack.pop()
            # possibly 32 pattern
            if stack and num > stack[-1][1]: # 132 pattern
                return True
            stack.append((num, mn)) # store 13 pattern
        mn = min(mn, num)
    return False

# tests
# [1,2,3]
# [1,3,2]
# [2,3,1] -> F
# [3,5,0,3,4], didn't cover
```

maintaining a decreasing stack eliminates unnecessary compares:

* 13 patterns are naturally stored \(with caching min value\)
* 32 pattern can be found by popping from the stack

bonus: approach \#3 in official solution

### 503. Next Greater Element II

associate the right values to left

solution: clever to deal with circular structure instead of copying -&gt; **modulo**, my original sol: **concat** two copy, tail to head

### 402. Remove K Digits

greedy: increasing stack, remove **21 pattern \(inverse consecutive pairs\)**

### 484. Find Permutation

greedy, take care of the `D*`  patterns

