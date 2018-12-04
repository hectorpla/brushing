# Examine termination

### 33. Search in Rotated Sorted Array

see the end

```python
def search(nums, target):
    # assumption: no duplicates
    lo, hi = 0, len(nums) - 1
    while lo <= hi: # termination cond
        mid = (lo + hi) // 2
        if target == nums[mid]:
            return mid
        # bug: >= !
        if nums[mid] >= nums[lo]: # determine the shapes of the left and right
            # bug: <= !!!
            if nums[lo] <= target <= nums[mid]: # determine target is on left or right
                hi = mid
            else:
                lo = mid + 1
        else:
            if nums[mid] <= target <= nums[hi]:
                lo = mid
            else:
                hi = mid - 1
    return -1
    
    # suspicious cases:
    # [1,2], 1 ==> line 6
    # [1,2], 2 ==> line 14
    # [2,1], 2 ==> line 14
    # [2,1], 1 ==> line 6
    # [1], 0 ==> line 6
    # [1], 2 ==> line 6
```



