# Corner cases

### 162. Find Peak Element

```python
def findPeakElement(self, nums):
    # corners: problem on line 6
    if len(nums) < 2 or nums[0] > nums[1]:
        return 0
    
    # we set lo to 1, what if the answer is 1? guard!
    lo, hi = 1, len(nums) - 1
    
    # find the last increasing consecutive pair
    while lo < hi:
        mid = (lo + hi + 1) // 2
        # assume the neighbor values are different
        if nums[mid-1] < nums[mid]:
            lo = mid
        else:
            hi = mid - 1
    return lo
    
    # tests
    # [1]
    # [1,2]
    # [2,1], bug covered
    # [1,2,3]
    # [3,2,1]
    # [1,3,2]
```

