# Matrix traverse

### 498. Diagonal Traverse

!!!! corner cases, the top-right and the bottom-left

**not** good to **greedily** update \(i, j\), rewrite it!

```python
def findDiagonalOrder(self, matrix):
    m, n = len(matrix), len(matrix[0]) if matrix else 0
    
    # two types of turns: 
    # 1. up -> down:  
    #   a. i < 0
    #   b. j = n
    # 2. down -> up
    #   a. j < 0
    #   b. i = m
    dirs = [[-1,1], [1,-1]]
    
    result = []
    i, j = 0, 0
    d = 0
    while len(result) < m * n:
        # turnings
        # bugs: top-right corner!!! order matters!!!
        if j == n:  # check j first because the top-right corner is included into this case
            assert d == 0
            i, j = i + 2, j - 1
            d = 1
            continue
        if i < 0:
            assert d == 0
            i += 1
            d = 1
            continue
        if i == m:
            assert d == 1
            i, j = i - 1, j + 2
            d = 0
            continue
        if j < 0:
            assert d == 1
            j += 1
            d = 0
            continue
        
        # invariant: i in [0, m), j in [0, n)
        assert 0 <= i < m and 0 <= j < n
        result.append(matrix[i][j])
        di, dj = dirs[d]
        i, j = i + di, j + dj
        
    return result

# tests
# []
# [[]]
# [[1]]
# [[1,2,3]] # single row
# [[1],[2],[3]] -- single col, test top-right corner
# [[1,2,3],[4,5,6]] -- bottom-left corner
# [[1,2,3],[4,5,6],[7,8,9]]
```

### 

### 54. Spiral Matrix

mostly hard-coded

think about extendability \(tentative\):

1. starting point \(top-left, bottom-left, etc.\)
2. direction: clockwise or anti-clockwise

```python
def spiralOrder(self, matrix):
    m, n = len(matrix), len(matrix[0]) if matrix else 0
    result = []
    
    # bookkeep boundaries
    upperRow, lowerRow = 0, m - 1
    leftCol, rightCol = 0, n - 1
    
    dirs = [[0,1],[1,0],[0,-1],[-1,0]] # r, d, l, u
    i, j = 0, 0  # the current position
    d = 0
    while len(result) < m * n:
        assert upperRow <= i <= lowerRow and leftCol <= j <= rightCol  # invariant
        result.append(matrix[i][j])
        
        # maintain the invariant: rules to turn 
        # hard coded, should be more programmatic so that we can change the traverse order
        if d == 0 and j == rightCol:
            d = 1
            upperRow += 1
        elif d == 1 and i == lowerRow:
            d = 2
            rightCol -= 1
        elif d == 2 and j == leftCol:
            d = 3
            lowerRow -= 1
        elif d == 3 and i == upperRow:
            d = 0
            leftCol += 1
        di, dj = dirs[d]
        i, j = i + di, j + dj
    return result
# tests
# []
# [[]]
# [[1]]
# [[1,2]]
# [[1,2,3]] - 1*3
# [[1],[2]]
# [[1],[2],[3]] - 3*1, r -> d
# [[1,2],[3,4]] - 2*2, d -> l
# [[1,2,3],[4,5,6]] -- 2*3
# [[1,2,3],[4,5,6],[7,8,9]] -- 3*3, l -> u
# [[1,2,3,4],[5,6,7,8],[9,10,11,12],[13,14,15,16]]
```

### 885. Spiral Matrix III

DRY: code reuse -&gt; **multiplexer - demultiplexer** technique: collapse dimensions, see the `walkThru()` function

```python
def spiralMatrixIII(self, R, C, r0, c0):
    dirs = [(0,1), (1,0), (0,-1), (-1,0)]  # r, d, l, u
    def endOf(i0, j0, k, d):
        vectors = [
            [-k, k+1],
            [k+1, k+1],
            [k+1, -k-1],
            [-k-1, -k-1]
        ]
        vi, vj = vectors[d]
        return i0 + vi, j0 + vj
    
    def walkThru(i0, j0, iEnd, jEnd):
        """ only one dimension changes, so many tricky places """
        # 1. decide dimension: i or j
        di, dj = iEnd - i0, jEnd - j0
        assert (di == 0) ^ (dj == 0)
        
        # 2. decide direction: increase or decrease
        if di == 0 and not 0 <= i0 < R:
            return
        if dj == 0 and not 0 <= j0 < C:
            return
        # multiplexer: map two dimension into one: only one needs change
        if dj == 0:
            dx = di
            x0, xEnd = (i0, iEnd) 
            limitD = R
        else:
            dx = dj
            x0, xEnd = (j0, jEnd)
            limitD = C
        step = -1 if dx < 0 else 1
        if dx < 0:
            start, end = min(x0 + step, limitD-1), max(xEnd, 0) - 1
        else:
            start, end = max(x0 + step, 0), min(xEnd, limitD-1) + 1
        for k in range(start, end, step):
            # de-multiplexer
            yield [i0, k] if di == 0 else [k, j0]
        # end
        
    # assume first coord is inBound
    result = [[r0, c0]]  # the initials
    i, j = r0, c0
    k = 0
    
    while len(result) < R * C:
        for d, (di, dj) in enumerate(dirs):
            endI, endJ = endOf(r0, c0, k, d)
            # print((i,j), '->', (endI, endJ), 'd:', (di, dj))
            
            # TODO: walk thru the line
            result.extend(walkThru(i, j, endI, endJ))
            
            i, j = endI, endJ
        k += 1
    return result
```



