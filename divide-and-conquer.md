# Divide & Conquer

## quick sort

Partition \(very very tricky, **distinct** keys or not\)

### 215. Kth Largest Element in an Array

背答案吧：line 11,  15,  17, 20~21,  22

```python
def findKthLargest(nums, k):
    # quick select
    def partition(lo, hi):
        """return int in [lo, hi]
        """
        # optimize: pick a better pivot
        pivot = nums[lo]
        
        i, j = lo + 1, hi
        # bug2: !!! foggy termination condition, unclear advance logic
        while i <= j: # ! two pointers should cross
            # loop invariant: all number on the left (inclusive) to j is <= pivot
            #                 all number on the right to j is >= pivot
            
            while i <= hi and nums[i] <= pivot: # bug4: i < hi
                i += 1
            while j > lo and nums[j] >= pivot: # bug3: j > lo + 1
                j -= 1
            # print(i, j)
            if i < j: # if cross, don't exchange
                nums[i], nums[j] = nums[j], nums[i]
        nums[lo], nums[j] = nums[j], nums[lo]
        return j
    
    # tests
    # [1], 1 -> 1
    # [1,2], 1 -> 2
    # [1,2], 2 -> 1
    # [2,1], 1 -> 2
    # [2,1], 2 -> 1
    
    # [1,2,3], 1
    # [1,2,3], 2
    # [1,2,3], 3
    def select(lo, hi):
        # print(nums, lo, hi)
        targetIndex = len(nums) - k
        pivotIndex = partition(lo, hi)
        
        if pivotIndex == targetIndex:
            # print(nums, pivotIndex)
            return nums[pivotIndex]
        if targetIndex < pivotIndex: # bug1: symbol reversed
            return select(lo, pivotIndex - 1)
        return select(pivotIndex + 1, hi)
    return select(0, len(nums) - 1)

```

