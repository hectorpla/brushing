# Reverse



```python
def swapPairs(head):
    
    # recursive
    if head is None or head.next is None:
        return head
        
    first, second, tail = head.next, head, head.next.next
    first.next = second
    second.next = self.swapPairs(tail)
    return first

def swapPairs(head):
    # iterative: just convert the recursive ver.
    dummy = ListNode(-1)
    dummy.next = head
    head = dummy
    
    while head.next and head.next.next:
        first, second, rest = head.next.next, head.next, head.next.next.next # lengthy but understandable, draw picture
        head.next = first
        first.next = second
        second.next = rest
        # three nodes changed, checked
        head = second
    
    return dummy.next
# tests
# []
# [1]
# [1,2]
# [1,2,3]
# [1,2,3,4]
```



### 25. Reverse Nodes in k-Group

```python
def reverse(head):
        prev = None
        while head:
            nxt = head.next
            head.next = prev
            prev, head = head, nxt # bug: didn't reset prev
    
def getKNext(head, k):
    for _ in range(k):
        if head is None:
            return None
        head = head.next
    return head

# recursive
def reverseKGroup(self, head, k):
    def reverseRest(head):
        kNextPrev = getKNext(head, k - 1)
        if kNextPrev is None:
            return head
        kNext = kNextPrev.next
        kNextPrev.next = None # cut the link between k-1st and kth
        reverse(head)
        head.next = reverseRest(kNext)
        
        return kNextPrev
    
    return reverseRest(head)

def reverseKGroup(self, head, k):
    # iterative
    dummy = ListNode(-1)
    dummy.next = head
    prev = dummy
    
    while prev:
        kNextPrev = getKNext(prev, k)
        if kNextPrev is None:
            prev = None
            break
        # cut the links
        head = prev.next
        prev.next = None
        kNext = kNextPrev.next
        kNextPrev.next = None
        
        # reverse & redirect pointers
        reverse(head)
        prev.next = kNextPrev
        head.next = kNext
        
        # advance to next iteration
        prev = head
    
    return dummy.next
```

