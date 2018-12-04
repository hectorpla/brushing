# Maze I II III

499

this algo not working:

```text
failing case, H: hole, B: ball
1 0 H 1
0 0 0 0
0 0 0 1
0 1 0 0
B 0 0 0

it takes 5 steps to go to (1,2) - go up first, but facing right
it takes 7 steps to go to (1,2) - go right first, this time we can get to the hole
```

```python
from collections import deque
class Solution:
    def findShortestWay(self, maze, ball, hole):
        m, n = len(maze), len(maze[0]) if maze else 0
        dirs = [[-1,0],[0,1],[0,-1],[1,0]] # up, right, left, down
        dirSymbols = ['u','r','l','d']
        def isBound(i, j):
            nonlocal m, n
            return not (0 <= i < m and 0 <= j < n) or maze[i][j] == 1
        
        si, sj = ball
        gi, gj = hole
        # BFS
        q = deque([(si, sj, d) for d in range(4)])
        location2from = {(si,sj): None} # {(i,j): int}
        goalAchieved = False
        while q:
            levelLocation2from = {} # {(i,j): int}, a subset of location2from
            # level traversal
            for _ in range(len(q)):
                i, j, d = q.popleft()
                # calculate all possible next states
                di, dj = dirs[d]
                ii, jj = i + di, j + dj
                if not isBound(ii, jj): # bug!!!: not inBound
                    children = [(ii, jj, d)]
                else:
                    children = []
                    for k, (di, dj) in enumerate(dirs):
                        ii, jj = i + di, j + dj
                        if not isBound(ii, jj):
                            children.append((ii, jj, k))
                
                # BFS next level
                for ch in children:
                    ii, jj, d = ch
                    # once the parent of a place is marked, it is done
                    if (ii, jj) in location2from:
                        continue
                    # annot parent
                    levelLocation2from[ii, jj] = d
                    # check goal
                    if (ii, jj) == (gi, gj):
                        goalAchieved = True
                    q.append(ch)
            location2from.update(levelLocation2from) # update the path to points we reached at this level
            if goalAchieved:
                break
        
        if (gi, gj) not in location2from:
            return "impossible"
        result = []
        ci, cj = gi, gj
        while location2from[ci, cj] is not None: # track the path back
            d = location2from[ci, cj]
            sym = dirSymbols[d]
            if not result or sym != result[-1]:
                result.append(sym)
            di, dj = dirs[d]
            ci, cj = ci - di, cj - dj
        return ''.join(reversed(result))
```



A new algo: like word ladder II, first BFS and then DFS

