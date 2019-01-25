# Bipartite

run thru tests using call stack

```python
def possibleBipartition(N, dislikes):
    """
    :type N: int
    :type dislikes: List[List[int]]
    :rtype: bool
    """
    
    def construct():
        graph = [[] for _ in range(N)]
        for p1, p2 in dislikes:
            graph[p1-1].append(p2-1) # bug: pairs are 1-based
            graph[p2-1].append(p1-1)
        return graph
    graph = construct()
    
    # tests
    # 1, [] -> True
    # 2, [] -> True
    # 2, [[0,1]] -> True
    # 3, [[0,1], [1,2]] -> True
    # 3, [[0,1], [1,2], [2, 0]] -> False
    # 4, [[0,1], [1,2], [2,3], [3,0]] -> True
    # 4, [[0,1], [1,2], [2,3], [3,0], [0,2]] -> False, *run
    # two components
    # 3, [[1,2]] -> True
    # 4, [[1,2],[2,3]] -> True
    # 4, [[1,2],[2,3],[3,1]] -> False
    
    # call stack
    # current-node label  neighbors      groups
    # 0              0      [1,3,2]      [0,-1,-1,-1]
    # 1              1      [0,2]        [0,1,-1,-1]
    # 0              0, ok
    # 2              0      [1,3]        [0,1,0,1]
    # 1              1, ok
    # 3              1      [2,0]        [0,1,0,1]
    # 2              1 but get 0
    
    groups = [-1] * N
    def traverse(start, label):
        """mark the current node as `label` 
        and check if its neighbors have the other label 
        """
        if groups[start] != -1: # avoid re-visit
            return True
        groups[start] = label
        for neighbor in graph[start]:
            if groups[neighbor] == label:
                return False
            traverse(neighbor, label ^ 1)
        return True
    
    # all dfs-trees in the forest should be OK
    return all(map(lambda p: traverse(p, 0), range(N)))
```

