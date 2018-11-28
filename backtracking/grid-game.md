# Grid Game

### 51. N-Queens

```python
def solveNQueens(self, n):
    """
    :type n: int
    :rtype: List[List[str]]
    """
    
    # hash for cell
    cols = [False] * n
    diag1 = [False] * (2 * n - 1)
    diag2 = [False] * (2 * n - 1)
    
    results = []
    # state: (work, start_row)
    work = []
    def search(row):
        if row == n:
            results.append(work.copy())
            return
        for col in range(n):
            d1 = col-row+n-1 # [-n + 1, n - 1]
            d2 = col+row # [0, 2 * n - 2]
            if cols[col] or diag1[d1] or diag2[d2]:
                continue
            cols[col], diag1[d1], diag2[d2] = True, True, True
            work.append(col) # bug: didn't add
            candidate = search(row + 1)
            work.pop()
            cols[col], diag1[d1], diag2[d2] = False, False, False
    
    def constructGrid(assignment):
        if not assignment:
            return None
        return ['.' * assignment[i] + 'Q' + '.' * (n - assignment[i] - 1) for i in range(n)]
    
    search(0)
    return list(map(constructGrid, results))
```

