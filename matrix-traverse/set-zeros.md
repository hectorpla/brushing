# Set zeros

clear phases:

1. store states for row 0 and col 0
2. get zeros from matrix\[1:, 1:\] and store them in row heads and col heads
3. set zeros with those info
4. set zeros for row 0 and col 0

```python
def setZeroes(self, matrix):
    """
    :type matrix: List[List[int]]
    :rtype: void Do not return anything, modify matrix in-place instead.
    """
    
    # m[0][0] -> m[0], m[:][0]; m[0][j] -> m[:][j], m[i][0] -> m[i][:]
    
    m, n = len(matrix), len(matrix[0]) if matrix else 0
    if m * n == 0:
        return
    
    # tests
    # [[1]] -> [[1]]
    # [[1,0]] -> [[0,0]]
    # [[1],[0]] -> [[0],[0]]
    # [[1,0],[1,1]] -> [[0,0], [1,0]]
    row0 = int(all(num != 0 for num in matrix[0])) # bug: didn't convert type
    col0 = int(all(matrix[i][0] != 0 for i in range(m)))
    
    for i in range(1, m):
        for j in range(1, n):
            if matrix[i][j] == 0:
                matrix[i][0], matrix[0][j] = 0, 0
                
    for i in range(1, m):
        for j in range(1, n):
            matrix[i][j] = matrix[i][0] and matrix[0][j] and matrix[i][j]
    
    for j in range(n):
        matrix[0][j] = row0 and matrix[0][j]
        
    for i in range(m):
        matrix[i][0] = col0 and matrix[i][0]

```

