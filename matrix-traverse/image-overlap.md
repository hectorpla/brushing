---
description: '835'
---

# Image Overlap

not very straight forwards, implementation is tedious



decreasing the dimension helps with decreasing the program complexity



My solution using python generator:

```python
from collections import deque
def largestOverlap(self, A, B): 
    def count_overlap(A, B):
        # assume the shapes are the same
        result = 0
        for row_a, row_b in zip(A, B):
            result += sum(a and b for a, b in zip(row_a, row_b))
        return result
    
    # test
    # [[1,0], [0,1]], [[0,0], [1,0]]
    def shifted_matrices(M):
        """generate shifted matrices shifting (0, 0) to (n-1, n-1), 
        the bottom right corner  """
        n = len(M)
        zero_row = [0] * n
    
        matrix = deque(map(deque, M))
        for _ in range(n): # column iter
            for _ in range(n): # row iter
                yield matrix
                last_row = matrix[n-1]
                last_row.pop() # shift right
                last_row.appendleft(0)
                matrix.appendleft(zero_row) # shift down
            # now the matrix is of size (2*n, n)
            for _ in range(n): # restore the matrix
                matrix.popleft()
    
    result = 0
    for a in shifted_matrices(A):
        result = max(result, count_overlap(a, B))
    for b in shifted_matrices(B):
        result = max(result, count_overlap(A, b))
    return result
```



