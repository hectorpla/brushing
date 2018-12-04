# Skyline problem



### 218. The Skyline Problem

O\(n^2\)

{% hint style="info" %}


Invariant: 

```python
all heights in heightPile are valid before appending a new [x, y] pair
```
{% endhint %}

```python
def getSkyline(self, buildings):
    points = []
    for lo, hi, height in buildings:
        points.append((lo, height))
        points.append((hi, -height))
    points.sort()
    
    n = len(points)
    
    result = []
    heightPile = [] # unsorted
    i = 0
    # same process as the heap solution
    while i < n:
        x = points[i][0]
        while i < n and points[i][0] == x: # mind the termination!!!
            _, height = points[i]
            # every height pushed once and popped once
            if height < 0:
                index = heightPile.index(-height) # remove from the pile
                del heightPile[index]
            elif height > 0:
                heightPile.append(height) # add to the pile
            i += 1
        # invariant: all heights in heightPile are valid
        y = max(heightPile) if heightPile else 0
        if not result:
            result.append([x, y])
            continue
        lastX, lastHeight = result[-1]
        if y != lastHeight: # bug: [[1,2,1],[2,3,1]]?
            result.append([x,y])
            
    return result

# tests
# [[1,2,1], [1,2,2]], overlapped start
# [[1,2,1],[2,3,1]], unchanged heights
# [[1,2,3], [5,6,8]]
```



O\(nlogn\) solution from Leetcode server:  
1. collect all the important points, and scan them from left to right on the axis   
2. maintain a heap to keep track of the highest height ever

```python
def getSkyline(self, buildings):
    corners = sorted({b[0] for b in buildings} | {b[1] for b in buildings})
    print(corners)
    j = 0
    res = [(0,0)]
    heap = [(0,float('inf'))]
    heapq.heapify(heap)
    for x in corners:
        while j < len(buildings) and buildings[j][0] == x:
            heapq.heappush(heap,(-buildings[j][2],buildings[j][1]))
            j += 1
        while heap[0][1] <= x:
            heapq.heappop(heap)
        if -heap[0][0] != res[-1][1]:
            res.append((x, -heap[0][0]))
    return res[1:]
```



