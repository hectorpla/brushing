# Hash table problems

### 128. Longest Consecutive Sequence

can be done with **Union-find**

clean code: line 10~11

```text
def longestConsecutive(self, nums):
    # hash table
    
    num2range = {} # {int: (int, int)}
    result = 0
    
    for num in nums:
        if num in num2range:
            continue
        lo = num2range[num-1][0] if num - 1 in num2range else num
        hi = num2range[num+1][1] if num + 1 in num2range else num
        num2range[num] = (lo, hi)
        if lo != num:
            num2range[lo] = (lo, hi)
        if hi != num:
            num2range[hi] = (lo, hi)
        result = max(result, hi - lo + 1)
    return result
            
    # tests
    # [1] -> 1
    # [1,3] -> 1
    # [1,2,4] -> 2
    # [2,4,3] -> 3
    # [2,5,8,3,9,4,7] -> 4
```

