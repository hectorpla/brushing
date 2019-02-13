---
description: 'June, Dec 18, Feb 19'
---

# Two Pointers

### **15. 3sum**

ensure unique: make sure the first and the second number are unique

```python
def threeSum(self, nums):
    n = len(nums)
    
    # 1. sort
    nums.sort()
    
    # 2. fix first (distinct) and two-pointer to find (second, first) pairs
    results = []
    
    for i, first in enumerate(nums):
        if i and first == nums[i-1]:
            continue
        j, k = i + 1, n - 1
        while j < k:
            second, third = nums[j], nums[k]
            # invariant: j should be the rightmost index for `second` and k should be the leftmost for `third`
            if j > i + 1 and second == nums[j-1]: # bug: first condition
                j += 1
                continue
            if k < n - 1 and third == nums[k+1]:
                k -= 1
                continue
            su = first + second + third
            if su == 0:
                # deduplicated second and third
                results.append([first, second, third])
                j += 1
                k -= 1
            elif su < 0:
                j += 1
            elif su > 0:
                k -= 1
            else:
                assert False # cannot reach here
    return results
    # tests
    # []
    # [1,2]
    # [-1,0,1]
    # [1,2,3]
    # [-1,-1,2]
    # [-2,1,1]
    # [-1,-1,0,1]
    # [-2,-1,0,1,2]
```

interesting solution from the sever \(refer to **Leetcode**\):

```python
    def threeSum(self, nums):
        # count each number's frequency
        freq = defaultdict(int)
        for n in nums:
            freq[n] += 1
        results = set() # for deduplication
        
        # corner case
        if 0 in freq and freq[0] > 2:
            results.add((0,0,0))
                
        # property: 
        #      of the 3 numbers, at least 1 positive and 1 negative
        positives = [p for p in freq if p > 0]
        negatives = [n for n in freq if n < 0]
        
        for p in positives:
            for n in negatives:      
                other = - p - n # leftover
                
                if other in freq: 
                    # known: freq[other] must>0
                    
                    # if other == p or n, we need to make sure there are at least 2 other
                    if other == p and freq[p] > 1:
                        results.add((n, p, p)) # order can be known
                    elif other == n and freq[n] > 1:
                        results.add((n, n, p)) # order can be known  
                    elif n < other < p:
                        results.add((n, other, p))

        return list(results)
```

### 215. Kth Largest Element in an Array

take-away: **invariant!**

```python
def findKthLargest(self, nums, k):
    # quick select, skip randomization
    k -= 1 # bug: 0-based index
    def findKth(start, end):
        if start == end:
            assert start == k
            return nums[k]
        index = partition(start, end)
        if k == index: # bug: !!!
            return nums[k]
        if k > index:
            return findKth(index + 1, end)
        return findKth(start, index-1)
    
    def partition(start, end): # return the pivot index after partitioning the sub-array
        # do-it in-place
        # guard against negative index; overlap index
        assert start >= 0 and end < len(nums) and start < end 
        i, j = start, end
        
        while i <= j: # check termination
            # invariant: 
            # nums[z] >= nums[start] for z in [start, i) 
            # and nums[z] < nums[start] for z in (j, end]
            if nums[i] >= nums[start]:
                i += 1
            elif nums[j] < nums[start]:
                j -= 1
            else: # exhaustive
                assert (nums[i] < nums[start] 
                        and nums[j] >= nums[start])
                nums[i], nums[j] = nums[j], nums[i]
                i += 1
                j -= 1
        assert i > j
        # now j is the last position that > nums[start]
        nums[start], nums[j] = nums[j], nums[start]
        return j
    
    return findKth(0, len(nums) - 1)
# tests
# [1], 1
# [2,1], 1
# [2,1], 2
# [3,1,2], 1
# [3,1,2], 2
# [3,1,2], 3
# [3,2,3,1,2,4,5,5,6], 4 -- given
```

### 75. Sort Colors

```python
def sortColors(self, nums: 'List[int]') -> 'None':
    # invariant, i, j, k: i <= j <= k, 
    # nums[o] == 0 for o in [0, i), 
    # nums[o] == 2 for o in (k, n-1]
    # nums[0] == 1 for o in [i, j)
    
    n = len(nums)
    i, j, k = 0, 0, n - 1
    while j <= k:
        if nums[j] == 0:
            nums[i], nums[j] = nums[j], nums[i]
            i += 1
            assert nums[j] < 2
            j += 1
        elif nums[j] == 2:
            nums[j], nums[k] = nums[k], nums[j]
            k -= 1
        else:
            j += 1
    # tests
    # [0]
    # [2,1]
    # [1,0,2]
    # [2,1,0]
```

### 360. Sort Transformed Array

ends to middle vs. middle to ends

