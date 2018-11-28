# 3 sum

```python
def threeSum(self, nums):
    """
    :type nums: List[int]
    :rtype: List[List[int]]
    """
    
    # unique triplets
    # sort + two pointers
    
    n = len(nums)
    nums.sort()
    results = []
    
    # tests
    # [-1,-1,2] # didn't cover
    # [-2,1,1,2], triple containing duplicates
    # [-2,0,0,2], duplicate second
    # [-2,0,2,2], duplicate third
    # [-5,-5,2,3], duplicate first
    
    target = None
    for i, first in enumerate(nums):
        if -first == target: # potential bug: didn't care for the first elem
            continue
        target = -first
        j, k = i + 1, n - 1
        
        while j < k:
            su = nums[j] + nums[k]
            if (j > i + 1 and nums[j] == nums[j-1]) or su < target: # bug: should not check first == second
                j += 1
            elif su == target:
                results.append([first, nums[j], nums[k]])
                j += 1
            else:
                k -= 1
    return results
```

