# Matrix

## Test recipe

single row/col tests for boundary cases

## Problems

### 174. Dungeon Game

{% hint style="info" %}
RR: hp\(i, j\): the lowest HP of the **best** path \(with highest lowest HP\) starting from \(i, j\)

finally, we add 1 to make sure the HP will never drops down to `1`

caveat: boundary case, **line 7**
{% endhint %}

```python
def calculateMinimumHP(self, dungeon):
    # rr: hp(i, j) = min(0, dungeon[i][j] + max(hp(i+1,j), hp(i,j+1))), 
    # which is non-positive
    m, n = len(dungeon), len(dungeon[0]) if dungeon else 0
    
    hp = [float('-inf')] * (n + 1)
    hp[n-1] = 0 # bug: initially all bottoms should be -inf except n-1
    
    # fill from bottom to top, right to left
    for i in range(m - 1, -1, -1):
        for j in range(n - 1, -1, -1):
            # print(dungeon[i][j], hp[j], hp[j+1])
            hp[j] = min(0, dungeon[i][j] + max(hp[j], hp[j+1])) # typo: j -> i
    return -hp[0] + 1
    
    # tests
    # [[-1],[-2],[0]], single row
    # [[-1],[-2],[0]] single col
```

