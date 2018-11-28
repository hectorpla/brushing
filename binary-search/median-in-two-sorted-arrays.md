# Median in two sorted arrays

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

