# Queue/Heap Problem

### 846. Hand of Straights

two traps: 1. simply thought sorting solves the problem; 2. bug of control flow

```python
def isNStraightHand(self, hand, W): 
    # [1], 1 -> True
    # [1], 2 -> False: good catch for wrongly returning True At the end
    # [1,2], 2 -> True
    # [1,3], 2 -> False
    # [1,2,2,3], 2 -> True
    # [1,2,2,3,3,4] -> False # failed the first time, trap 1

    counts = collections.Counter(hand)
    keys = [k for k in counts]
    heapq.heapify(keys)

    count = 0
    prev = None
    for _ in range(len(hand)): # bug: termination while keys
        if count == 0:
            prev = keys[0]
        elif counts[prev+1] < 1:
            return False
        else:
            prev += 1
        counts[prev] -= 1
        if counts[prev] == 0:
            heapq.heappop(keys) # bug: used pop, # bug: should only do pop 
        count = (count + 1) % W
    return count == 0
```

**Follow Up （**[**https://leetcode.com/problems/hand-of-straights/discuss/135598/C++JavaPython-O\(MlogM\)-Complexity**](https://leetcode.com/problems/hand-of-straights/discuss/135598/C++JavaPython-O%28MlogM%29-Complexity)**）**  
We just got lucky AC solution. Because `W <= 10000`.  
What if W is huge, should we cut off card on by one?





### 358. Rearrange String k Distance Apart

Essences: 

1. constrain that in every slot no repeating tasks, should delay the re-heappush process
2. break loop: when the number of tasks is less than k, should break the **outer** loop
3. case k == 0 

```python
def rearrangeString(self, s, k):
    # bug3: corner case k = 0
    if k == 0: return s
    
    counts = Counter(s)
    hq = []
    for task, freq in counts.items():
        heapq.heappush(hq, (-freq, task))
    result = []

    # arrange tasks in unit of slots, a slot is of size k + 1 
    # and there are no recurring chars in same slot

    # priority queue: element sorted according to frequency and alphabetical order

    # "aabbcc", 3 -> abcabc
    # "abb", 3 -> ""
    # "aaadbbcc", 2 -> abcabcda
    # instance 1: [(0, a), (0, b), (0,c)], abcabc
    # instance 2: [(1,b), (0,a)], ba_
    while hq:
        removed = []
        for _ in range(k):
            if not hq:
                hq.clear()
                # bug2: caused by fixing bug2, should guarantee, when failed, the hq is clear
                removed.clear() 
                break
            rank, task = heapq.heappop(hq)
            result.append(task)
            count = -rank - 1
            removed.append((count, task)) # bug1: didn't delay the pushing
        for count, task in removed:
            if count > 0:
                heapq.heappush(hq, (-count, task))

    return ''.join(result) if len(result) == len(s) else ""
```

