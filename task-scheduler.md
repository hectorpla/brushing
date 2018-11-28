# Task Scheduler

### 621. Task Scheduler



### 767. Reorganize String

proposing strategies \(greedy\): keep the countings \# as balanced as possible

```python
from heapq import heappush, heappop, heapify
from collections import Counter
def reorganizeString(self, S):
    # consider cases:
    # 1. a: 6, b: 3, c: 3
    # 2. a: 3, b: 3, c: 3
    
    # always pop pairs with largest countings
    
    freq = Counter(S)
    pq = [(-fq, char) for char, fq in freq.items()] # ensure lexi order
    heapify(pq)
    
    n = len(S)
    result = []
    for i in range(n // 2):
        if len(pq) == 1: # the only char has no others to pair with
            return ""
        fq1, char1 = heappop(pq)
        fq2, char2 = heappop(pq)
        result.extend([char1, char2])
        if fq1 < -1:
            heappush(pq, (fq1 + 1, char1))
        if fq2 < -1:
            heappush(pq, (fq2 + 1, char2))
            
    # bug: ignore
    if pq: # for odd number of chars
        result.append(pq.pop()[1])
    return ''.join(result)

# tests
# "aba"
# "abaa"
# "aaabbbccc"
# "aaaaaabbbccc"
# "aaaaaabbbcc"
# "aaaaaabbbc"
```

