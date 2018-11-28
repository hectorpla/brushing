# Interval problems

###  pro370. Range Addition

scan-line, O\(n + klogk\), while the optimal is O\(n + k\)

### 128. Longest Consecutive Sequence

```python
def longestConsecutive(self, nums):
        num2range = {} # {int: (int, int)}
        result = 0
        
        for num in nums:
            if num in num2range:
                continue
            lo = num2range[num-1][0] if num - 1 in num2range else num
            hi = num2range[num+1][1] if num + 1 in num2range else num
            num2range[num] = (lo, hi)
            if lo != num:
                num2range[lo] = (lo, hi)
            if hi != num:
                num2range[hi] = (lo, hi)
            result = max(result, hi - lo + 1)
        return result                
        # tests
        # [1] -> 1
        # [1,3] -> 1
        # [1,2,4] -> 2
        # [2,4,3] -> 3
        # [2,5,8,3,9,4,7] -> 4
```

### 判断一个数组是不是连续的\[1 , 2, 3, 5 , 4\]连续， \[1, 0 , 3, \]连续。 0可以匹配任何元素。这个提出了几个做法。最后的大哥给的提示解很trick。比较最大最小非0元素的差和数组长度。

### 352. Data Stream as Disjoint Intervals

```java
class SummaryRanges {
    private TreeMap<Integer, Interval> intervals;

    public SummaryRanges() {
        intervals = new TreeMap<>();
    }
    
    /**
    * careful: might be duplicates
    */
    public void addNum(int val) {
        Interval prev = null; // val - 1 might be inserted before, but not as key
        Map.Entry<Integer, Interval> lower = intervals.lowerEntry(val + 1);
        if (lower != null) {
            // sanity check: whether val was inserted
            if (lower.getValue().end >= val) {
                return;
            } else if (lower.getValue().end == val - 1) {
                prev = intervals.remove(lower.getKey());
            }
        }
        
        Interval suc = intervals.remove(val + 1);
        
        int start = prev != null ? prev.start : val;
        int end = suc != null ? suc.end : val;
        
        Interval intv = new Interval(start, end);
        intervals.put(start, intv);
    }
    
    public List<Interval> getIntervals() {
        return new LinkedList<Interval>(intervals.values());
    }
}
```

### 218. The Skyline Problem

O\(n^2\), 



O\(nlogn\) solution from Leetcode server:  
1. collect all the important points, and scan them from left to right on the axis   
2. maintain a heap to keep track of the highest height ever

```python
def getSkyline(self, buildings):
    corners = sorted({b[0] for b in buildings} | {b[1] for b in buildings})
    print(corners)
    j = 0
    res = [(0,0)]
    heap = [(0,float('inf'))]
    heapq.heapify(heap)
    for x in corners:
        while j < len(buildings) and buildings[j][0] == x:
            heapq.heappush(heap,(-buildings[j][2],buildings[j][1]))
            j += 1
        while heap[0][1] <= x:
            heapq.heappop(heap)
        if -heap[0][0] != res[-1][1]:
            res.append((x, -heap[0][0]))
    return res[1:]
```

### 435. Non-overlapping Intervals

```python
def eraseOverlapIntervals(self, intervals):
    # like greedy: unweighted course can take (criteria: earlist end time)
    # can be done by DP, sort according to end time, calculate the max # non-overlapping intervals of the schedule including         # intervals[i].end as the end or ealier, O(n^2) or O(nlogn) with binary search
    result = 0
    
    intervals.sort(key=lambda intv: intv.end)
    
    prev = float('-inf')
    for intv in intervals:
        if intv.start < prev: # bug, reversed sign
            result += 1
        else:
            prev = intv.end
    return result

# tests
# [[1,2],[2,3],[3,4],[1,3]]
# [[1,5],[1,3],[2,3],[6,7]]
```



### 436. Find Right Interval

sort according to start, and use heap to keep track of least end

top sol used TreeMap, less space \(compared to two hashtable in my solution\)

```python
def findRightInterval(self, intervals):
    # idea: record start->index mapping, record end points (for less space), sort by start point
    # end -> start -> index
    end2start = {}
    start2index = {intv.start:i for i, intv in enumerate(intervals)}
    ends = [intv.end for intv in intervals]
    
    # use a priority queue to store: ends are not sorted
    q = []
    intervals.sort(key=lambda intv: intv.start)
    for intv in intervals:
        while q and q[0] <= intv.start:
            end2start[heappop(q)] = intv.start
        heappush(q, intv.end)        
    
    for i, end in enumerate(ends):
        ends[i] = start2index.get(end2start.get(end, float('inf')), -1)
    return ends

# tests
# [[1,2]]
# [ [3,4], [2,3], [1,2] ]
# [ [1,4], [2,3], [3,4] ], cases with encompassing intervals
```



