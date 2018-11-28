# 错题集

### 366. Find Leaves of Binary Tree \(BFS approach\)

### 

### 621. Task Scheduler

greedy, thought it too complicated

### 787. Cheapest Flights Within K Stops

功夫没到家，实际上可以BFS做，但是半路转成A\*  
different approaches, completely different cost to debug codes  
A\* is incorrect  
3 Approaches

**Take-away: in python, no block scope!  no error when overriding variables, use function instead**

Each approach is twisted towards the goal: A\*, BFS  


### 130. Surrounded Regions

so many bugs: 1. check\_valid\(\); 2. flood, flood partially \(break loop\) 3. when to set cell value

### 279. Perfect Squares

BFS very slow, can DFS do better, DP?

### 

```python
def permuteUnique(self, nums):
    # following the idea from Permutation I
    n = len(nums)
    # tests: 
    # [1,1] -> [[1,1]]
    # [1,1,2] -> [112, 121, 211]
    # [2,1,1] -> didn't test; bug

    # duplication: skip the swaps that swap two same number
    results = []
    def swap_and_search(start):
        nonlocal n
        if start >= n:
            results.append(nums.copy())

        met = set()
        for i in range(start, n):
            # should swap the first different number: make sure the all subtrees are unique
            if nums[i] in met:
                continue
            met.add(nums[i])
            prev = nums[i]
            nums[i], nums[start] = nums[start], nums[i]
            swap_and_search(start + 1)
            nums[i], nums[start] = nums[start], nums[i]
    swap_and_search(0)
    return results
```



## 1457. Search Subarray

#### Description

Given an array `arr` and a positive integer `k`, you need to find a continuous array from this array so that the sum of this array is `k`. Output the length of this array. If there are multiple such arrays,return the output starts with the smallest position, if there are more than one, the output ending position is the smallest.If no such sub-array can be found, return -1.

use inclusive prefix sum \(normally we use exclusive sum\), so should take care of the 

```python
def searchSubarray(self, arr, k):
    # positive elements, big assumption
    # can incrementally add tail to the window; when overflow, pop from head
    
    # !trap: should output smallest start point first, and smallest end point second
    # actually not a trap: we examine each end point in the outer loop, so if there
    # is no possible subarray for this end point, we move on, the first subarray
    # we encount is the first one that in the array
    
    # OK, bad interpretation of the problem, elems can be negative
    # this case, not hard either, prefix sum + hash map
    
    acc = 0
    sum2index = {}
    sum2index[0] = -1 # bug2: othw, ignoring window starting at 0
    
    # tests
    # [0], 0 -> 1
    # [1,0,0,2,2], 2 -> 3
    # [1,-1,0], case failed in two trials
    for i, num in enumerate(arr):
        acc += num
        if acc - k in sum2index:
            return i - sum2index[acc - k] # bug1: bad math, should not + 1
        # do after goal check: prevent empty length window
        sum2index.setdefault(acc, i)
    return -1
```

