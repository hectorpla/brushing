# Subsequence

### LIS

### 368. Largest Divisible Subset

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

### 446. Arithmetic Slices II - Subsequence

unlike ver. I \(solved with formulae and cache\), this problem can be solved only using **counting technique**

{% hint style="info" %}
states: 

`(0: standalone) =>`   
`(1: has predecessor with respect to a difference) =>   
(2: >= 2 in the sequence, means forming seq with length >= 3)`

when we are at state 2, we know we can add the count in state 1 into the result
{% endhint %}

```python
def numberOfArithmeticSlices(self, A):    
    n = len(A)
    counts = [{} for _ in range(n)] # {(diff: int): int}
    
    result = 0
    for i, suc in enumerate(A):
        for j in range(i):
            pred = A[j]
            diff = suc - pred
            
            # 3 states: 
            
            # 0. stand-alone for the pred: hasPreds
            # 1. A[j] has predeccessor: hashPreds > 0
            hasPreds = counts[j].get(diff, 0)
            
            # convert all seqs with state 1 at index j into state 1 at index i
            # the extra `1` is for the stand-alone A[j] in state 0
            counts[i][diff] = counts[i].get(diff, 0) + hasPreds + 1
            
            # 2. has two or more preceding elems: seqs with the last two nums as A[j] and A[i] and A[j] has some preceding
            result += hasPreds
    return result

# tests
# [1,1,1]
# [1,1,1,1]
# [1,2]
# [1,2,3]
# [1,1,2,3], bug!!
# [1,2,2,3,3]
# [1,1,2,2,3,3]
# [2,4,6,8,10]
# [2,2,4,6,8,10]
# [2,2,4,6,8,8,10]
```

