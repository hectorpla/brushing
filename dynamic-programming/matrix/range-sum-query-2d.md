# Range Sum Query 2D

### 304. Range Sum Query 2D - Immutable

* **lazy** evaluation \(maybe not very handy to implement iteratively\); top sol: **greedy** initialization
* bug spot: **inclusive** or not
* iterative impl: padding for the two dimension

```python
class NumMatrix:
    
    # define sumRect(row, col): sumRegion(0, 0, row, col)
    # sumRect(row, col) = matrix[row][col] + sumRect(row - 1, col) + sumRect(row, col - 1) - sumRect(row - 1, col - 1)
    
    def __init__(self, matrix):
        """
        :type matrix: List[List[int]]
        """
        self.matrix = matrix
        self.m, self.n = len(matrix), len(matrix[0]) if matrix else 0
        self.sumRect = [[None] * self.n for _ in range(self.m)]
        
    def _calculateSumRect(self, row, col):
        if row == -1 or col == -1:
            return 0
        if self.sumRect[row][col] is not None:
            return self.sumRect[row][col]
        res = self.sumRect[row][col] = (self._calculateSumRect(row-1, col) 
                                      + self._calculateSumRect(row, col-1)
                                      - self._calculateSumRect(row-1, col-1)
                                       + self.matrix[row][col])
        return res
        
    def sumRegion(self, row1, col1, row2, col2):
        """
        :type row1: int
        :type col1: int
        :type row2: int
        :type col2: int
        :rtype: int
        """
        # sanity check
        if not (0 <= row2 < self.m and 0 <= col2 <= self.n):
            raise "invalid input"
        # exclusive !!! source of bug
        row1 -= 1
        col1 -= 1
        # ---------------
        # |area1 | area2|
        # |area3 | area4|
        # ---------------
        # area4 - area2 - area3 + area1
        return (self._calculateSumRect(row2, col2) 
                - self._calculateSumRect(row2, col1)
                - self._calculateSumRect(row1, col2) 
                + self._calculateSumRect(row1, col1))
```

