# Partition

### 86, 148: partition, sort list

Take-away:   
- should always think about dummy node dealing with LinkedList  
- O\(n\) average extra space \(draw recursion tree\), so there might no benefit than copying it first to array and sort

bugs: 

1. didn’t set tail.next to **null**;   
2. qsort: indefinite recursion due to possibly not decreasing size of sub problem

```python
def sortList(head): 
    """ :type head: ListNode :rtype: ListNode """ 
    def partition(head, x):
        dummy1 = ListNode(-1) 
        ltTail = dummy1 
        dummy2 = ListNode(-1) 
        geTail = dummy2
        while head:
            if head.val < x:
                ltTail.next, ltTail = head, head
            else:
                geTail.next, geTail = head, head
            head.next, head = None, head.next # BUG: should set next pointer NULL
        # ltTail.next = dummy2.next # don't connect at this stage
        return dummy1.next, dummy2.next

    # [1] -> 1
    # [3,1] -> [1,3]
    # [2,5,3] -> [2,3,5]
    def sort(head):
        """no randomization process though
           might first find the middle value and tail value,  
           take the mediate of [first, middle, tail] 
        """
        if head is None:
            return head, head

        head1, head2 = partition(head, head.val)

        sortedHead1, sortedTail1 = sort(head1)

        # partition function always leave the head to the second part
        # sort head2 is never null
        middleTail = head2
        while middleTail.next and middleTail.next.val == middleTail.val:
            middleTail = middleTail.next
        
        # should not call on head2: [1,2] -> [], [1,2] (indefinite recursion)
        sortedHead2, sortedTail2 = sort(middleTail.next) 

        dummy = ListNode(-1) # good practice using dummy
        tail = dummy
        if sortedHead1:
            tail.next, tail = sortedHead1, sortedTail1
        tail.next, tail = head2, middleTail
        if sortedHead2:
            tail.next, tail = sortedHead2, sortedTail2
        return dummy.next, tail

    return sort(head)[0]
```

