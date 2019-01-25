# BFS

## Recipes

to prevent bugs, ensure that: 

1. whether the operations are done for **node** or **edge** **perspective**
2. whether the operations are done in the current level or the next level

### 133. Clone Graph

recipe:  **node** creation is done in child\(**next**\) level

java idiom: side effect in lambda in computeIfAbsent

```java
public class Solution { 
    public UndirectedGraphNode cloneGraph(UndirectedGraphNode node) { 
        if (node == null) return null; 
        Map label2clone = new HashMap<>(); 
        Queue q = new LinkedList<>();
        q.offer(node);
        while (!q.isEmpty()) {
            UndirectedGraphNode cur = q.poll();
            // only the root is absent
            UndirectedGraphNode clone = 
                label2clone.computeIfAbsent(cur.label, (l) -> new UndirectedGraphNode(l));
    
            for (UndirectedGraphNode neighbor: cur.neighbors) {
                int label = neighbor.label;
                UndirectedGraphNode clonedNeighbor = 
                    label2clone.computeIfAbsent(label, (l) -> {
                        // !!! clever: every created node is appended to the queue only once
                        q.offer(neighbor);
                        return new UndirectedGraphNode(l);
                    });
                clone.neighbors.add(clonedNeighbor);
            }
        }
        return label2clone.get(node.label);
    }
}
```



### 785. Is Graph Bipartite?

recipe: check **edge** between current node and its neighbor **\(next level\)**

```python
def isBipartite(self, graph):
    def flood(start): # return True if the constraints maintained
        q = deque([start])
        cur_label = 0
        while q:
            for _ in range(len(q)):
                node = q.popleft()
                node2label[node] = cur_label
                for neighbor in graph[node]:
                    if neighbor not in node2label:
                        q.append(neighbor)
                    elif node2label[neighbor] == cur_label:
                        return False
            cur_label ^= 1
        return True

    # tests
    # [[1],[]] -> True
    # [[1],[2],[0]] -> False
    # [[1,2],[3],[4],[],[]]  -> True
    for i in range(len(graph)):
        if i not in node2label and not flood(i): # bug: didn't check if visited
            return False
    return True
```

DFS: check odd-length cycle

