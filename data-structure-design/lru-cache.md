# LRU cache

take-away: pre-condition and post-condition

```python
class LRUCache:
    class ListNode():
        def __init__(self, key, value):
            """node in a doubly linkedlist"""
            self.key, self.val = key, value
            self.prev, self.next = None, None
            
        def detach(self):
            # bug: wrongly value set
            prev, nxt = self.prev, self.next
            prev.next, self.prev = nxt, None
            self.next, nxt.prev  = None, prev

    def __init__(self, capacity):
        """
        :type capacity: int
        """
        self.capacity = capacity
        self.nodeFinder = {} # {int: ListNode}
        # prev: tail, next: head; head: least recently used, tail: most recently used
        anchor = self.anchor = self.ListNode(-1, -1) # the anchor of a `cyclic` doubly linkedlist
        anchor.prev, anchor.next = anchor, anchor # missing

    def get(self, key):
        """
        :type key: int
        :rtype: int
        """
        nodeFinder = self.nodeFinder
        # 1. check in nodeFinder
        node = nodeFinder.get(key, None)
        if node is None:
            return -1
        
        # 2. update the postion of the node
        node.detach()
        self._append(node)
        
        assert self.anchor.prev is node
        return node.val

    def put(self, key, value):
        """
        :type key: int
        :type value: int
        :rtype: void
        """
        nodeFinder = self.nodeFinder
        # 1. check key in nodeFinder, if in, update value and position
        node = nodeFinder.get(key, None)
        if node is not None:
            node.val = value
            node.detach()
            self._append(node)
        
        # 2. new key, check size == capacity, if so expire the head in the list, update nodeFinder
        else:
            capacity, nodeFinder = self.capacity, self.nodeFinder
            if capacity == len(nodeFinder):
                self._expire()
                assert len(nodeFinder) == capacity - 1 # post-cond
            node = self.ListNode(key, value)
            self._append(node)
            nodeFinder[key] = node
        assert node.val == value
        assert self.anchor.prev is node
    
    def _append(self, node):
        """append node at the tail"""
        anchor = self.anchor
        oldTail = anchor.prev
        oldTail.next, node.prev = node, oldTail
        node.next, anchor.prev = anchor, node
        
    def _expire(self):
        """remove the head from the list and update the nodeFinder """
        head = self.anchor.next
        assert head.key in self.nodeFinder
        head.detach()
        del self.nodeFinder[head.key] # bug: head.key -> head.val

# Your LRUCache object will be instantiated and called as such:
# obj = LRUCache(capacity)
# param_1 = obj.get(key)
# obj.put(key,value)
```

