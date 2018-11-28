# Flood Filling

## Flood Filling

### 417. Pacific Atlantic Water Flow \(doesn't care level\)

starting from the borders

### 323. **Number of Connected Components in an Undirected Graph**

Two approaches: 1. DFS/BFS search, 2. union-find

trade-offs:   
search algo should: 1. construct the graph from the list of edges; 2. maintain a hashmap to prevent cycle  
union-find: easy initialization with arrays, relatively simple find/union operations

```python
def countComponents(self, n, edges): 
    """ :type n: int :type edges: List[List[int]] :rtype: int """
    # time/space: O(n)
    
    # 1, [] -> 1
    # 2, [[0,1]] -> 1
    # 4, [[0,1], [2,3]] -> 2
    # 4, [[0,1], [2,3], [0,2]] -> 1

    # did not encapsulate the data structure
    def find(p):
        while id_[p] != p:
            id_[p] = id_[id_[p]]
            p = id_[p]
        return p

    def union(p, q):
        proot, qroot = find(p), find(q)
        if proot == qroot:
            return False
        if sz[proot] >= sz[qroot]:
            id_[qroot] = proot
            sz[proot] += sz[qroot]
        else:
            id_[proot] = qroot
            sz[qroot] += sz[proot]
        return True

    id_ = [i for i in range(n)]
    sz = [1] * n
    result = n

    for p, q in edges:
        if union(p, q):
            result -= 1

    return result
```

DFS: 1 construction, 2 search

```java
class Solution { 
    // 1, [] -> 1 
    // 2, [0,1] -> 1
    // 3, [[0,1]] -> 2 
    public int countComponents(int n, int[][] edges) { int result = 0;    
        List<List<Integer>> graph = constructGraph(n, edges);
        Set<Integer> visited = new HashSet<Integer>();
        for (int i = 0; i < n; i++) {
            if (fill(graph, i, visited)) {
                result++;
        }
    }
    return result;
}

    private List<List<Integer>> constructGraph(int n, int[][] edges) {
        List<List<Integer>> graph = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            graph.add(new ArrayList<Integer>());
        }
        for (int[] edge: edges) {
            int src = edge[0], dest = edge[1];
            graph.get(src).add(dest);
            graph.get(dest).add(src);
        }
        return graph;
    }
    
    private boolean fill(List<List<Integer>> graph, int start, Set<Integer> visited) {
        if (visited.contains(start)) return false;
    
        visited.add(start);
        for (int neighbor: graph.get(start)) {
            fill(graph, neighbor, visited);
        }
        return true;
    }
}
```

### 

