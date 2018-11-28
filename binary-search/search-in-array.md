# Search in array

### 153. Find Minimum in Rotated Sorted Array

always model the problem into **000011111**  
0: nums\[i\] &gt;= nums\[lo\]  
1: nums\[i\] &lt; nums\[lo\]  
find the first **1**

```python
def findMin(self, nums):
    # run manually
    # [1] -> 1
    # [1,2] -> 1
    # [2,1] -> 1
    # [3,1,2] -> 1
    # [2,3,1] -> 1

    lo, hi = 0, len(nums) - 1
    while lo < hi:
        mid = (lo + hi) // 2
        if nums[mid] < nums[lo]: 
            hi = mid
        elif nums[lo] < nums[hi]: # shape: sorted
            hi = mid
        else: # shape: rotated
            lo = mid + 1

    return nums[lo] if nums else None # in case nums is empty
```

another approach: first check the **shape** then the middle value



### 34. Search for a Range

corner case: empty array

```python
def searchRange(self, nums, target):
    def find_first():
        lo, hi = 0, len(nums) - 1
        while lo < hi:
            mid = (lo + hi) // 2
            if nums[mid] >= target:
                hi = mid
            else:
                lo = mid + 1
        return lo if nums[lo] == target else -1

    def find_last():
        lo, hi = 0, len(nums) - 1
        while lo < hi:
            mid = (lo + hi + 1) // 2
            if nums[mid] <= target:
                lo = mid
            else:
                hi = mid - 1
        return lo if nums[lo] == target else -1

    # [], 1 -> [-1,-1] # bug
    # [1], 1 -> [0,0]
    # [1,3],2 -> [-1,-1]
    # [1,3],3 -> [1,1]
    if len(nums) == 0:
        return [-1, -1]
    return [find_first(), find_last()]
```

