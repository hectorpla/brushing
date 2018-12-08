# Try and error \(value search\)

### 719. Find K-th Smallest Pair Distance

{% hint style="info" %}
find the first number x: has count\(diff &lt;= x for diff in all diffs\) &gt;= k
{% endhint %}

```python
def smallestDistancePair(self, nums, k):
    nums.sort()
    n = len(nums)
    
    # matrix[i][j] = nums[j] - nums[i], increasing in rows, decreasing in columns
    # nums = [1,2,3,4,5]
    # 1 2 3 4
    #   1 2 3
    #     1 2
    #       1
    # naive solution: search from the top right corner with a heap
    
    # binary search
    lo, hi = 0, nums[-1] - nums[0]
    
    def check(x):
        nonlocal n
        acc = 0
        # two pointers (sliding window)
        left = 0
        for right in range(1, n):
            while nums[right] - nums[left] > x:
                left += 1
            acc += right - left
        return acc >= k
        # or even using binary search if not comming up with sliding window
        
    # *find the first number x: has count(diff <= x for diff in all diffs) >= k, a little bit tricky here
    # 1 3 5 7, k = 2
    # lo hi
    #  1  7
    #  1  4
    #  2  4
    #  3  4
    #  3  3
    while lo < hi:
        mid = (lo + hi) // 2
        if check(mid): # check count
            hi = mid
        else:
            lo = mid + 1
    return lo

# tests
# [1,2], 1
# [1,2,3], 1
# [1,2,3], 2
# [1,2,3], 3, bug discovered
# [1,2,3,4], 1 -> 1
# [1,2,3,4], 4 -> 2
# [1,2,3,4], 6 -> 3
# duplicates
# [1,1], 1 -> 0
# [1,1,2], 1 -> 0
# [1,1,2], 2 -> 1
# [1,1,2], 3 -> 1
# [1,1,1], 3 -> 0
```

