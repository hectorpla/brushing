# With level limit

### 787. Cheapest Flights Within K Stops



![a, b, c](../../.gitbook/assets/cheapest_flight_k_stop4.png)

\(a\) should eliminate re-visit after a cycle, there is no point to do so \(prevent memory explosion\)  
\(b\) with level limit, we will only reach the node we can reach  
\(c\) don't need to explore the node twice; we can pick the path with less cost \(update min distance in next level\)

**Recipe \(wrong\):** update min distance in advance \(in next level/**edge**\), check whether we should continue on the **current** level \(mind the stoping level\)

Above: wrong !!!!!

\(c\) should not update the min distance in current level

```python
def findCheapestPrice(self, n, flights, src, dst, K):
    # problem statement: given a directed graph, find the min cost within K + 2 level
    # to clarify: not much

    # construct graph
    graph = [[] for _ in range(n)]
    cost = {} # {int: {int: int}}
    def construct_graph(): # prevent var name crashes
        for src, dest, val in flights:
            graph[src].append(dest)
            cost.setdefault(src, {})[dest] = val

    construct_graph()
    # init
    min_dist = [float('inf')] * n

    result = float('inf')
    q = deque([(src, 0)]) # (node: int, distance: int), where the distance is that's acc so far
    # recipe: update the min distance from src in the next node
    for _ in range(K + 2): # min_dist updates to level K + 1 because updates are done in next level
        for _ in range(len(q)):
            node, dist = q.popleft()
            if dist > min_dist[node]: # greater distance with more/equal steps, safely eliminate
                continue
            min_dist[node] = dist
            for neighbor in graph[node]:
                dist2neighbor = dist + cost[node][neighbor]
                # min_dist[neighbor] = min(min_dist[neighbor], dist2neighbor) # bug
                q.append((neighbor, dist2neighbor))
    return min_dist[dst] if min_dist[dst] < float('inf') else -1
```

