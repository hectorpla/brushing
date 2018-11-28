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



### 403. Frog Jump

{% hint style="info" %}
cross\(i, k\): starting from position i, with the past step as k, can the frog reach the last stone

caveat: should keep in mind that the frog should jump forward \(k &gt; 0\)

cross\(i, k\) = k - 1 and cross\(i+k-1, k-1\) or k and cross\(i+k, k\) and cross\(i+k+1, k+1\)  
base case: cross\(i, k\) = True if i == n-1, cross\(i, k\) == False if i not in stones

also BFS works
{% endhint %}

