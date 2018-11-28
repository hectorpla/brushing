# Corner cases

### 265. Paint House II

n = 0 -&gt; 0  
n = 1 -&gt; min\(costs\[0\]\)  
n &gt; 1, k &lt; 2 -&gt; inf  
otherwise, dp

```python
def minCostII(self, costs):
        def findSmallestTwo(nums):
            # return index of the smallest two
            if len(nums) == 1:
                return [0, 0]
            result = [0, 1] if nums[0] < nums[1] else [1, 0]
            for i, num in enumerate(nums[2:], start=2):
                if num < nums[result[1]]:
                    result[1] = i
                    if nums[result[1]] < nums[result[0]]:
                        result[0], result[1] = result[1], result[0]
            return result
        
        # idea, care only same color or not,
        n, k = len(costs), len(costs[0]) if costs else 0
        if n == 0:
            return 0
        if n > 1 and k < 2:
            return 2 ** 31 - 1 # max_int in java
        dp = [[float('inf')] * k, [0] * k]
        this, other = 0, 1
        
        for cost in costs:
            c1, c2 = findSmallestTwo(dp[other])
            for color, val in enumerate(cost):
                dp[this][color] = val + dp[other][c1 if color != c1 else c2]
            this, other = other, this
        return min(dp[other])
        
        # tests
        # []
        # [[1]]
        # [[1,1],[2,2]] -> 3
        # [[1,2],[1,3]] -> 3
        # [[1,3],[2,5],[1,9]] -> 7
        # [[1,5,3],[2,9,4]] -> 5
```



### 288. Unique Word Abbreviation

1. duplicates in list
2. word to check doesn't exist in the pre-given dictionary

### 163. Missing Ranges

corner: duplicates!!!

```text
def findMissingRanges(self, nums, lower, upper):
    # corner, duplicates!!!
    start = lower
    result = []
    nums.append(upper + 1) # don't forget the last missing range that reaches upper
    for num in nums:
        if num < start:
            continue
        if num == start:
            start += 1
            continue
        if num == start + 1:
            result.append(str(start))
        else:
            result.append("{}->{}".format(start, num-1))
        start = num + 1
    nums.pop()
    return result
```

