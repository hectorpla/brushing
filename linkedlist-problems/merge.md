# Merge



* 2 Add two numbers:
* 21 Merge two sorted list: merge until either of the heads is None, then concat
* 23 Merge k lists

```python
# failed program
def mergeKLists(self, lists):
    # [[1], []] -> [1]
    # [[2], [1]] -> [2,1]
    # [[2], [1], [0,3]] -> [0,1,2,3]
    # [[1], [1]] -> [1,1] # duplicate key
    for head in lists:
        if head:
            heapq.heappush(hq, (head.val, head)) # ListNode is not comparable

    dummy = ListNode(-1)
    tail = dummy
    while hq:
        _, head = heapq.heappop(hq)
        tail.next, tail = head, head
        if head.next:
            heapq.heappush(hq, (head.next.val, head.next))
        head.next = None
    return dummy.next
```

