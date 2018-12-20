---
description: 'July, Dec'
---

# Median in two sorted arrays

```python
def findMedianSortedArrays(self, nums1, nums2):
    n1, n2 = len(nums1), len(nums2)
    def findKth(start1, start2, k):
        """
        [1,3,5], [2,4]:
            k = 1 => 1
            k = 2 => 2
            k = 3 => 3...
        """
        nonlocal n1, n2
        # print(start1, start2, k)
        assert k > 0 # 1-indexed 
        if start1 == n1:
            return nums2[start2 + k-1] # bug: forgot the offset
        if start2 == n2:
            return nums1[start1 + k-1]
        if k == 1:
            return min(nums1[start1], nums2[start2])
        # want to rule out the k // 2 elems
        # compare the k / 2 - 1st elem for both arrays
        # k, compares
        # 2,  0
        # 3,  0
        # 4,  1
        thrownSize = k // 2
        assert thrownSize > 0 # otherwise, inifinite loop
        mid1 = start1 + thrownSize - 1
        mid2 = start2 + thrownSize - 1
        midVal1 = nums1[mid1] if mid1 < n1 else float('inf')
        midVal2 = nums2[mid2] if mid2 < n2 else float('inf')
        
        if midVal1 < midVal2:
            return findKth(start1 + thrownSize, start2, k - thrownSize)
        return findKth(start1, start2 + thrownSize, k - thrownSize)
    
    n = n1 + n2
    mid = n // 2
    mean2 = findKth(0, 0,  mid + 1)
    # print(mid + 1, mean2)
    if n & 1:
        return mean2
    mean1 = findKth(0, 0, mid)
    # print(mid, mean1)
    return (mean1 + mean2) / 2
        
# tests
# [1], []
# [1,2],[]
# [1],[2]
# [1,2],[3,4]
# [1,3],[2,4]
# [2],[1,3,4] # bug....
```



how many elements to eliminate each time

```python
def findMedianSortedArrays(nums1, nums2): 
    # [1], [] -> 1
    # [1,3], [] -> 2
    # [1], [3] -> 2
    # [1,2,3], [] -> 2
    # [1,2], [2] -> 2
    # [1,2,3,4], [] -> 2.5
    # [1,2,3], [4] -> 2.5

    def find_kth(k):
        lo1, lo2 = 0, 0
        while k > 1: # 1-based
            delta = k // 2 # number of elements to throw away: delta >= 1, critical
            mid1, mid2 = lo1 + delta - 1, lo2 + delta - 1
            if mid1 >= len(nums1):
                lo2 = mid2 + 1 # should advance at least 1
            elif mid2 >= len(nums2):
                lo1 = mid1 + 1
            elif nums1[mid1] < nums2[mid2]:
                lo1 = mid1 + 1
            else:
                lo2 = mid2 + 1
            k -= delta
        return nums1[lo1] if lo2 >= len(nums2) or\
            lo1 < len(nums1) and nums1[lo1] < nums2[lo2] else nums2[lo2]

    m, n = len(nums1), len(nums2)
    # total length -> k1, k2
    # 1 -> 1, 1
    # 2 -> 1, 2
    # 3 -> 2, 2
    # 4 -> 2, 3
    k1, k2 = (m + n + 1) // 2, (m + n + 2) // 2
    return (find_kth(k1) + find_kth(k2)) / 2
```

