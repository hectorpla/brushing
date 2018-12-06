# monotonic queue

## Recipe

actually an advanced version of monotonic **stack**, except that it allows the **popleft** operation, should be called a **deque**

* _**invariant 1**_: eliminate history **irrelevant** data using the stack property, data in the deque is **time sorted**
* _**invariant2**_**: expire** data at the tail, ensure that every datum is inserted into the deque once and popped from the deque once \(either via pop\(\) or popleft\(\)\)

## Problems

### 239. Sliding Window Maximum

non-increasing queue \(maintain the necessary info inside the window\)

1. add - multiple popleft and one push
2. remove - 0 or 2 popleft

```python
def maxSlidingWindow(self, nums, k):
    winTail = 0
    
    q = deque()
    result = []
    for i, num in enumerate(nums):
        # invariant 1
        while q and num > nums[q[-1]]:
            q.pop()
        q.append(i)
        
        if i - winTail + 1 == k:
            result.append(nums[q[0]]) # now the head of the queue is the max
            # # invariant 2: popleft
            if q[0] == winTail: # ensure each index is popped once
                q.popleft()
            winTail += 1
    return result
```

### 683. K Empty Slots

increasing queue, k-size window, min Queue to maintain the earliest blooming day in the window

```python
def kEmptySlots(self, flowers, k):
    # ... preprocessing logic
    result = float('inf')
    dq = deque()
    # maintain a window of size k
    for rightBound, day in enumerate(pos2day):
        # invariant: increasing property
        while dq and day < dq[-1]:
            dq.pop()
        dq.append(day)
        
        leftBound = rightBound - k + 1
        if leftBound - 1 >= 0 and rightBound + 1 < n\
                and dq[0] > pos2day[leftBound-1]\
                and dq[0] > pos2day[rightBound+1]:
            result = min(result, max(pos2day[leftBound-1], pos2day[rightBound+1]))
            
        # invariant: popleft to maintain the size of the window
        if leftBound >= 0 and pos2day[leftBound] == dq[0]:
            dq.popleft()
    return result if result != float('inf') else -1
```

