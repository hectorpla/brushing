# BFS pitfall

exponentially growing queue

### 529. Minesweeper

```python
def updateBoard(self, board, click):
    m, n = len(board), len(board[0])
    
    def inBound(i, j):
        nonlocal m, n
        return 0 <= i < m and 0 <= j < n
    
    near = [(di, dj) for di in range(-1,2) for dj in range(-1,2)]
    def minesAround(i, j):
        nonlocal m, n
        return sum(board[i+di][j+dj] == 'M' for di, dj in near
                   if inBound(i+di, j+dj))
    def explore(si, sj):
        q = deque([(si, sj)])
        
        while q:
            print(len(q), q)
            for _ in range(len(q)):
                i, j = q.popleft()
                
                mines = minesAround(i, j)
                if mines:
                    board[i][j] = str(mines)
                    continue
                board[i][j] = 'B'
                for di, dj in near:
                    ii, jj = i + di, j + dj
                    if not inBound(ii, jj) or board[ii][jj] != 'E': # bug: typo
                        continue
                    # if minesAround(ii, jj) == 0: # ????????????? why !!!! q size exponentially grows
                    #     board[ii][jj] = 'B'
                    q.append((ii, jj))
        
    i, j = click
    if board[i][j] == 'M':
        board[i][j] = 'X'
        return board
    
    explore(i, j)
    return board
```

if we mark a place only after it is popped \(without line 30 ~ 31\), the queue size will grow exponentially, very bad

```text
E E E E E
E E E E E
E E E E E
E E E E E
click on (2,2)

* number means the count in the queue

first layer of BFS
E E E E E
E 1 1 1 E
E 1 B 1 E
E 1 1 1 E

second
1 2 3 2 1
2 B B B 2
3 B B B 3
2 B B B 2

extend a row before the first row:
third
3 5 7 5 3 <- extend
B B B B B
B B B B B
B B B B B
B B B B B
```

However, with DFS,

