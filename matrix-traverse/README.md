# Matrix Traverse

**54. Spiral Matrix**

tried to traverse by rings, for example:

123; 456; 789 -&gt; 12369874, 5

the **motivations** to do this are: 1. don't check boundary; 2. more functional

But there are **corner cases** if we traverse **4 times** for every ring, like \[\[1\]\]. Be sure what you are doing

Two approaches to tackle the problem \(official solution\): simulation \(with directions switching\) and traverse by layers





### 174. Dungeon Game

```python
def calculateMinimumHP(self, dungeon):
    # [[0]] -> 1

    lo, hi = 0, 0

    for row in dungeon:
        hi += max(0, -min(row))

    def can_reach_with(health):
        # incorrect: greedy not always go on the path with smallest health
        m, n = len(dungeon), len(dungeon[0])
        i, j = 0, 0
        while i < m - 1 or j < n - 1:
            health += dungeon[i][j]
            if health < 0: # bug!!
                return False
            if i == m - 1:
                j += 1
            elif j == n - 1:
                i += 1
            elif dungeon[i+1][j] > dungeon[i][j+1]:
                i += 1
            else:
                j += 1
        return health + dungeon[i][j] >= 0 # bug: forget the last step!!!!!!!!!

    while lo < hi:
        mid = (lo + hi) // 2
        if can_reach_with(mid):
            hi = mid
        else:
            lo = mid + 1
    return lo + 1
```

