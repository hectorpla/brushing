# Improvement by making one small step

### Next permutation

analysis to generate the smallest next: the easy idea is to sort the remaining, but take a step further, at what stage we have come? from right to left, we get a non-decreasing sequence, **reverse** might work given this constraint

```python
def nextPermutation(self, nums):
    """
    :type nums: List[int]
    :rtype: void Do not return anything, modify nums in-place instead.
    """
    n = len(nums)
    
    def helperNext(start):
        index = start + 1
        for i in range(start + 1, len(nums)):
            if nums[i] > nums[start] and nums[i] <= nums[index]:
                index = i 
        nums[start], nums[index] = nums[index], nums[start]
        nums[start+1:] = list(reversed(nums[start+1:])) # improvement
    
    for i in range(n - 1, 0, -1):
        if nums[i] > nums[i-1]:
            helperNext(i-1)
            return
    nums[:] = nums[:][::-1]
    
    # tests
    # 123 -> 132
    # 132 -> 213
    # 321 -> 123
    # 255332 -> 323355, duplicates, important
```

