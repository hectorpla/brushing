---
description: 'Jun, Dec'
---

# Tree Related



### 133. Clone Graph

Input is given by a **node**, implying that we deal with a connected component, a **tree-like structure with backward edges;** On the other hand, if the input is given by adjacent matrix, no need to copy

* DFS, most straight-forward, post-order, first solve dependencies and then the current node
* BFS, have to create a node first and fill its neighbors at deeper levels, one less buggy solution: do two passes, first create node, second filling

### 207. Course Schedule \(**210 Course Schedule II**\)

Initially convert the edge collection into adjacent lists, 

* DFS: construct dependencies graph: dependant -&gt; dependency \(same direction of the definition, top-down\)
* BFS: construct successor graph: prerequisite -&gt; children \(the other way around\)
* 
```python
def canFinish(numCourses, prerequisites):
    """
    :type numCourses: int
    :type prerequisites: List[List[int]]
    :rtype: bool
    """
    # time/space: O(n)
    
    # 2, [[0,1]] -> True
    # 2, [[0,1], [1,0]] -> False
    
    # assumption: no duplicate edges -> simple graph
    # DFS
    dependencies = [[] for _ in range(numCourses)]
    
    for clss, dep in prerequisites:
        dependencies[clss].append(dep)
    
    # memoization
    solved = {}
    # path = []
    def solve_dep(clss):
        if clss in solved:
            return solved[clss]
        solved[clss] = False # clitical, prevent loop; assumption is correct
        for dep in dependencies[clss]:
            if not solve_dep(dep):
                break
        else:
            solved[clss] = True
            # TODO: for result requiring the topological sorting path
            # path.append(clss) all dependencies solved, add to the list safely
        return solved[clss]
    
    return all(solve_dep(clss) for clss in range(numCourses))
```



### 261. Graph Valid Tree

tricky to use: without using set to maintain met nodes, set children to None \(making use of the constructed data structure\)

```python
def validTree(self, n, edges):
    if n - 1 != len(edges):
        return False
    def constructGraph(): # easy to construct given nodes as consecutive numbers
        graph = [[] for _ in range(n)]
        for a, b in edges:
            graph[a].append(b)
            graph[b].append(a)
        return graph

    graph = constructGraph()
    # DFS
    stack = [0]
    while stack:
        node = stack.pop()
        if graph[node] is not None: # the value for a node is ever once non-None
            stack.extend(graph[node])
            graph[node] = None
            n -= 1
    return n == 0
```

Dec: 1. analysis - increase correctness 2. planning - avoid panic 3. tests - incremental - size small to large

```python
def validTree(self, n, edges):
    # undirected graph
    # tree properties:
    # 1. no cycle
    # 2. connected
    
    # 1. sanity check: len(edges) == n - 1, necessary condition for connectedness
    # important! assuming no duplicate edge
    if len(edges) != n - 1:
        return False
    
    # observation: if |V| = |E| + 1, either the graph is connected or there are cycles
    # cycle <=> disconnected
    
    # 2. detect cycle
    ## two approaches: a. unifon find b. search in graph
    ## search
    
    # 2.1. build graph (adjacency list), two-way edge (v->w, w->v)
    graph = [[] for _ in range(n)]
    for src, dest in edges:
        graph[src].append(dest)
        graph[dest].append(src)
    
    # 2.2. detect cycle: in undirected graphs, a tree is connected: can reach all others from any node
    # DFS: be careful of backwards edge to the parent
    visited = set()  # {int}
    def walk(start, parent):
        if start in visited:  # prevent exploring a node twices
            return
        visited.add(start)
        for suc in graph[start]:
            if suc != parent:  # no cycle of length 2 by definition
                walk(suc, start)
        
    walk(0, -1)
    return len(visited) == n

# tests
# 0, [] -- invalid
# 1, []
# 1, [[0,0]]  -- self-ref, actually valid
# 2, [[0,1]]
# 2, [] -- disconnected
# 3, [[0,1],[0,2]]
# 3, [[0,1]]
# 3, [[0,1],[1,2]]
# 3, [[0,1],[1,2],[2,0]]
# 4, [[0,1],[1,2],[2,0]] -- cycle and disconnected
# 4, [[0,1],[1,2],[2,3]]
# 6, [[0,5],[0,1],[0,2],[1,3],[3,4]]
# 6, [[0,5],[0,1],[1,5], [2,3],[3,4],[4,5]] -- two cycles
```

