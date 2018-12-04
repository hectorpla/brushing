# Longest Line of Consecutive One in Matrix

### 562. Longest Line of Consecutive One in Matrix

**mistake**: accumulate counts from all directions, mock @Aoran Nov.30, 2018

```python
def longestLine(self, M):
    m, n = len(M), len(M[0]) if M else 0
    
    prevCount = [[0,0,0] for _ in range(n+2)]
    curCount = [[0,0,0] for _ in range(n+2)]
    
    result = 0
    for i in range(m):
        row = 0
        for j in range(n):
            if not M[i][j]:
                curCount[j+1] = [0,0,0]
                row = 0
                continue
            # prone to bug
            topleft, top, topright = prevCount[j][0], prevCount[j+1][1], prevCount[j+2][2]
            curCount[j+1] = topleft+1, top+1, topright+1
            row += 1
            result = max(result, *curCount[j+1], row)
            
        curCount, prevCount = prevCount, curCount # alternate rows
    return result

# tests
# [] -> 0
# [[1]] -> 1
# [[1],[1]] -> 2
# [[1,1]] -> 2, pitfall here, python allow negative subscript,
# [[1,1,1]], didn't cover, caused bug
# [[1,0],[0,1]] -> 2
# [[0,1],[1,0]] -> 2
# [[0,0,1],[0,1,1],[1,0,0]] -> 3
# [[0,0,1],[0,1,0]] -> 2, added
# [[0,1],[1,0],[0,0]] -> 2, added
```

