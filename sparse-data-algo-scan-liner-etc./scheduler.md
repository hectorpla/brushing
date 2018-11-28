# Scheduler

not yet familiar

Greedy: in every frame, schedule the tasks with most counts

```python
def leastInterval(self, tasks, n):
    k = len(tasks) counts = Counter(tasks)
    hq = [] 
    for task, count in counts.items(): 
        heapq.heappush(hq, (-count, task))
    # ["A", "A"], 2 -> 4 # empty between
    # ["A", "A", "B"], 2 -> 4 # insert when possible
    # ["A","B","A"], 1 -> 3 # dense 
    # ["A", "A", "A", "B", "B", "C"], 2 -> 7 # mixed
    # ["A","A","A","B","C","D","E","F"], 8 -> 10 # didn't come up: dense
    n += 1
    result = 0
    while k:
        decrlist = []
        delta = 0
        for _ in range(min(n, len(hq))):
            decrlist.append(heapq.heappop(hq))
            k -= 1
            delta += 1
        # print(decrlist)
        for count, task in decrlist:
            count = -count
            if count > 1:
                heapq.heappush(hq, (-(count-1), task))
        result += delta if k == 0 else n
    return result
```

