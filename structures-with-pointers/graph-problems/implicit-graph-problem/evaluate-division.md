---
description: 399. Evaluate Division
---

# Evaluate division

Think: is it possible to construct \(directed\) spanning tree so that the traverse could be done in tree manner?  
propose: no, a spanning tree is represented as undirected graph which implies two-way edges between nodes

Approach: here used a backtracking method \(record visited **nodes**\); in x-code interview, used **edges** are recorded

Other than DFS/BFS, union-find can also solve the problem: 1. find connect components  2. one used 

```python
# updated: Oct 16, 2018
def calcEquation(equations, values, queries):
    # idea: 
    # 1. construct graph where edge a -> b represents a / b, 
    # then a -> b -> c => a / c = a/b * b/c, should be directed graph
    # 2. backtracking + visited map
    
    # corner:
    # 1. a / b = 0, no edge b -> a
    # 2. some node not in graph!!!
    
    graph = {} # {str: str[]}
    weights = {} # {str: {str: int}}
    def addEdge(num, deno, val):
        graph.setdefault(num, []).append(deno)
        weights.setdefault(num, {})[deno] = val
    
    def connect(num, deno, val):
        addEdge(num, deno, val)
        if val != 0:
            addEdge(deno, num, 1 / val)
    
    for (num, deno), val in zip(equations, values):
        connect(num, deno, val)
    
    visited = { node: False for pair in equations for node in pair} # a static map, avoid resizing
    def findPath(start, goal): # return int?
        if start == goal:
            return 1.0
        if goal in weights.get(start, ""): # optimization
            return weights[start][goal]
        if visited[start]:
            return None
        visited[start] = True
        result = None
        for suc in graph[start]:
            intermediate = findPath(suc, goal)
            if intermediate is not None:
                result = weights[start][suc] * intermediate # bug: directly return, didn't restore state!!!
                connect(start, goal, result)
                break
        visited[start] = False
        return result
    
    def compute(num, deno):
        return num in graph and deno in graph and findPath(num, deno) or -1.0
    return [compute(num, deno) for num, deno in queries]
```



spent long time; bug in compute **weight** of edge in union phase

```python
def calcEquation(self, equations, values, queries):
    def find(p):
        if p not in ids:
            ids[p] = p
            ratio[p] = 1
            return p
        cur = p
        while cur != ids[cur]:
            # compress: update ids and ratio
            ratio[cur] = ratio.get(cur, 1) * ratio.get(ids[cur], 1)
            ids[cur] = ids[ids[cur]] 
            cur = ids[cur]
        return cur

    def union(p, q, val):
        # mind the direction: ids[p] = q implies ratio[p] = ratio[p] * ratio[q]
        proot, qroot = find(p), find(q)
        if proot == qroot: # cycle
            return
        prootsize, qrootsize = sz.setdefault(proot, 1), sz.setdefault(qroot, 1)
        # simple algebra!!! (didn't compute it: bug)
        r =  get_ratio_to_root(q) / get_ratio_to_root(p) * val  # computed it wrong
        if prootsize <= qrootsize:
            ids[proot] = qroot
            sz[qroot] += prootsize
            ratio[proot] = r
        else:
            ids[qroot] = proot
            sz[proot] += qrootsize
            ratio[qroot] = 1 / r # reverse the link

    def get_ratio_to_root(p):
        acc = 1
        while p != ids[p]:
            acc *= ratio[p]
            p = ids[p]
        return acc

    # first to demonstrate the approach and why it works
    def compute(num, den):
        if num not in ids or den not in ids:
            return -1
        if find(num) != find(den):
            return -1
        return get_ratio_to_root(num) / get_ratio_to_root(den)

    for ((num, den), val) in zip(equations, values):
        union(num, den, val)
    return [compute(num, den) for num, den in queries]
```

