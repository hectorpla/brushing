---
description: 'Jul, Oct, Nov, Dec, Jan'
---

# Toposort

### 310. Minimum Height Trees

two ways to tackle: 1. from every node fanning out 2. from the outmost **contour** shrinking in

![start from 1-node vs. start from n-nodes](../../../../.gitbook/assets/310mht_roots.png)

two ways to perception: 1. from node, 2. from **edge**: remove edges

Nov: saw solution, new alternatives: find the longest path

### 207. Course Schedule

recipe:  remove **node** in the **current** level

```python
def canFinish(self, numCourses, prerequisites):
    # 1, [] -> True
    # 2, [[0,1]] -> True
    # 2, [[1,0], [0,1]] -> False
    # 3, [[1,0], [2,0]] -> True

    degree = [0] * numCourses
    graph = [[] for _ in range(numCourses)]

    for suc, pre in prerequisites:
        degree[suc] += 1
        graph[pre].append(suc)
    q = deque()
    for i in range(numCourses):
        if degree[i] == 0:
            q.append(i)
    while q:
        node = q.popleft()
        numCourses -= 1
        for suc in graph[node]:
            degree[suc] -= 1
            if degree[suc] == 0:
                q.append(suc)
    return numCourses == 0
```



DFS + memo approach:

take away: clever use of **all** and **any**

Better version, care more about test cases

```python
def canFinish(self, numCourses, prerequisites):
    # directed graph, given edges
    
    n = numCourses
    pre = [[] for _ in range(n)]  # record prerequisites of each course
    for course, pred in prerequisites:
        pre[course].append(pred)
    
    canFinish = [-1] * n  # -1: uncomputed, 0: not possible to finish, 1: OK
    # DFS: canFinish(v) = canFinish(u) for all u in preprequisiteOf(v)
    # search + memo approach
    # O(2|V| + |E|)
    def tryFinish(course):
        if canFinish[course] != -1:
            return canFinish[course]
        canFinish[course] = 0  # if a cycle introduces this course back, then 0
        res = all(tryFinish(dependency) for dependency in pre[course])
        canFinish[course] = res
        return res
    
    return all(tryFinish(i) == 1 for i in range(n))

# tests
# 0, []
# 1, []
# 2, [[0,1]]
# 2, [[0,1],[1,0]] - fail
# 3, [[1,0]]
# 3, [[0,1],[1,2],[2,0]] - cycle
# 3, [[0,1]] - disconnected
# 4, [[0,1],[2,3]] - two components
# 4, [[0,1],[1,2],[2,0]] - two components, cycle
# 5, [[0,1],[0,2],[4,3],[3,0]] - a larger tree, 0 is not the root, dependencies solved from lower of the tree
```

