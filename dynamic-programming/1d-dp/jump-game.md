# Jump game

### 55. Jump Game

```python
def canJump(self, nums): 
    # O(n)
    # keep track of the furthest reachable index
    
    furthest = 0
    for i, num in enumerate(nums):
        if furthest < i:
            return False
        furthest = max(furthest, i + nums[i])
        # can exit ealier
    return True

# tests
# [1,1,1] -> T
# [1,0,1] -> F
```

