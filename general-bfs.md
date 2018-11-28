# General BFS

### 297. perfect squares

### 45. Jump Game II

BFS in this case, every level covers its furthest index it can reach

For example, 

```text
[2,3,1,1,4]
```

level 0: 0~1, level 1: 1~2, level 2: 3~4

```python
def jump(self, nums):  
    # complexity, BFS: O(product(nums)) if not pruned, otherwise O(n^2); 
    # DP1: O(n^2); O(n) solution?
    
    # reachability guaranteed
    
    # saw solution: BFS, this solution kind of like BFS but not exactly
    n = len(nums)
    step = 0
    lo, hi = 0, 0 # ensure we never visit the same index ever twice
    while hi < n - 1:
        nextLo = hi + 1
        for i in range(lo, hi + 1):
            hi = max(hi, i + nums[i])
        step += 1
        lo = nextLo
    return step
```

