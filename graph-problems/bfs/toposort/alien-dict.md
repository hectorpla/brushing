# Alien dict

Two phases: 1. extract dependencies from the list of word; 2. solve topo order

corner cases: different lengths of words

the set-up is cumbersome:  
1. use two pointers to extract words starting with same char  
2. logic a little bit messy and all over the place, which is not good for extension and fix

```python
def alienOrder(words):
    # example
    # [
    #   "wrt",
    #   "wrf",
    #   "er",
    #   "ett",
    #   "rftt"
    # ]
    
    # run of the dependency extraction
    # [[('w', 'rt'), ('w', 'rf'), ('e', 'r'), ('e', 'tt'), ('r', 'ftt')]]
    # w -> e -> r
    # for same starts, push new relation to the queue
    # [[('r','t'), ('r','f')], [('r', ''), ('t', 't')]]
    # r -> t, r -> f
    # [[('t', ''), ('f', '')]]
    # t -> f
    
    # runtime can be improved not slicing words but bookkeeping pointers
    q = deque([[(w[0], w[1:]) for w in words if w]])  # all words should be non-empty
    # print(q)
    
    # 1. build dependency graph: logic not elegant but clear, too many `while` loops though
    graph = {} # char -> {char}
    for word in words:
        for char in word:  # bug: make sure all chars are keys in the graph
            graph.setdefault(char, set())
    while q:
        order = q.popleft()
        n = len(order)
        start = 0
        i = 0
        charOrder = []
        while i < n:
            # detect the same initial char
            while i < n and order[start][0] == order[i][0]:  # (intial, rest) = order[start]
                i += 1
            if i - start > 1:
                # push new relation to q
                newOrder = []
                for k in range(start, i):
                    _, rest = order[k]
                    if rest:
                        newOrder.append((rest[0], rest[1:]))
                if len(newOrder) > 1:
                    q.append(newOrder)
            charOrder.append(order[start][0])
            start = i # update start
        
        # add order to graph
        for j in range(len(charOrder) - 1):
            src, dest = charOrder[j], charOrder[j+1]
            assert len(src) == 1 and len(dest) == 1
            graph[src].add(dest)
    #     print('->'.join(charOrder))
    #     print(q)
    # print('constructed graph:', graph)
    
    # 2. resolve topological ordering
    q = deque([char for char in graph if not graph[char]])  # 0 in-degree
    result = []
    while q:
        suc = q.popleft()
        assert not graph[suc]  # dependencies are all solved for suc
        result.append(suc)
        # O(26) to find the predecessors
        preds = (pred for pred in graph if suc in graph[pred])
        for pred in preds:
            graph[pred].remove(suc)
            if not graph[pred]:  # becomes empty
                q.append(pred)
                
    return ''.join(reversed(result)) if len(result) == len(graph) else ''

# tests
# ["a", "ab", "b", "d"]
# ["a", "b", "a"]
# ["ad", "ac", "ad", "b"]  -- partially solvable
# cases not covered
# ["z","z"] -- 'z' not in key
# ["ab","adc"] -- 'c' not in key
```

