# LinkedList

### 117. Populating Next Right Pointers in Each Node II

{% hint style="info" %}
loop invariant: the current row is a linkedlist and the head is the dummy node
{% endhint %}

```python
def connect(self, root):
    # idea: assuming we have populated the nth layer, we can populate the n+1st layer with constant space
    
    # iteratively solve it: 1. outer loop: layer, 2. inner loop: nodes in the same layer
    # view each layer as a linkedlist, a dummy node for each layer to access the head of the row
    
    # 1. init
    dummy = TreeLinkNode(-1)
    dummy.next = root
    
    # 2. loop over layers
    while dummy.next:  # terminates when the next layer is empty
        # init the next layer
        nextDummy = TreeLinkNode(-1)
        tail = nextDummy  # points to the tail of next layer list
        
        cur = dummy.next
        # construct the next layer list
        while cur:
            # explore the children of current list node
            for child in [cur.left, cur.right]:
                if child:
                    tail.next, tail = child, child
            cur = cur.next
        dummy = nextDummy  # renew dummy
        
# tests
#  [1,2,3,4,null,null,5]
```

