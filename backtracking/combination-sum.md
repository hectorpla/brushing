# Combination sum

### 40. Combination Sum II

draw the DFS tree  
easy errors for forgetting something: line 20, line 21

clean logic: line 11, line 13

top sol: without visited map

```python
def combinationSum2(self, candidates, target):
    # dfs with visited vector
    results = []
    candidates.sort()
    
    n = len(candidates)
    visited = [False] * n
    work = []
    def traverse(remaining, start):
        nonlocal n
        if remaining == 0:
            results.append(work.copy())
        if remaining <= 0:
            return
        for i in range(start, n):
            if i > 0 and candidates[i-1] == candidates[i] and not visited[i-1]:
                continue
            visited[i] = True
            work.append(candidates[i])
            traverse(remaining - candidates[i], i + 1) # !!!logic error: i -> start
            work.pop() # bug, forget to pop
            visited[i] = False
            
    traverse(target, 0)
    return results

# tests
# [1,1], 2 -> [[1,1]]
# [1,1,1], 2 -> [[1,1]]
# [1,1,2,2,3,5] -> [[1,2,2],[1,1,3],[2,3],[5]]
```

