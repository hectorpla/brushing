# LIS

### LIS



### 376. Wiggle Subsequence

```python
def wiggleMaxLength(self, nums):
    # like LIS, DP with O(n^2) not hard to think about
    n = len(nums)
    wiggleN = [1] * n # wiggleN[i]: largest wiggle length ending at i where the difference between nums[i] and its pred < 0
    wiggleP = [1] * n
    
    for i in range(1, n):
        for j in range(i):
            if nums[i] > nums[j]:
                wiggleP[i] = max(wiggleP[i], 1 + wiggleN[j])
            elif nums[i] < nums[j]:
                wiggleN[i] = max(wiggleN[i], 1 + wiggleP[j])
    return max(*wiggleP, *wiggleN) if len(nums) else 0 # bug: empty input
```

