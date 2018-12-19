---
description: 'June, Dec'
---

# Two Pointers

### **15. 3sum**

ensure unique: make sure the first and the second number are unique

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

actually 3 pointers

loop invariants: left to lo are 0s, right to ho are 2s, left to mid are 0s or 1s; **mid &gt;= lo \(bug\)**  
post-condition: see the code

```python
def sortColors(self, nums):
    lo, mid, hi = 0, 0, len(nums) - 1
    
    # post conditions:
    # [0], lo = 1 > mid = 0
    # [1], m = 1 > lo = 0
    # [2], hi = -1 < mid = 0
    # [0,1], m = 2 > hi = 1
    # [0,1,1,2], lo = 1, hi = 2, m = 3
    
    while mid <= hi: # important '=' sign
        if nums[mid] == 0:
            nums[lo], nums[mid] = nums[mid], nums[lo]
            lo += 1
            mid += 1 # bug: elements to the left of mid <= 1
        elif nums[mid] == 2:
            nums[mid], nums[hi] = nums[hi], nums[mid]
            hi -= 1
        else:
            mid += 1
```

