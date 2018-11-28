# Two Pointers

**15. 3sum**

ensure unique: make sure the first and the second number are unique

### 

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

