# LRU

pre-/post- conditions for every functions, top to bottom: reduce the **overhead** in mind to maintain consistency

```python
class ListNode:
    def __init__(self, key, val):
        self.key = key
        self.val = val
        self.prev, self.next = None, None
        
    def getData(self):
        return self.key, self.val
    
    def detach(self):
        prev, nxt = self.prev, self.next
        prev.next, nxt.prev = self.next, self.prev


class LRUCache:
    def __init__(self, capacity: 'int'):
        assert capacity > 0
        self.cap = capacity
        self.itemFinder = {}  # {int: ListNode}
        self.anchor = anchor = ListNode(-1, -1)
        anchor.prev, anchor.next = anchor, anchor

    def get(self, key: 'int') -> 'int':
        itemFinder = self.itemFinder
        node = itemFinder.get(key)
        if not node:
            return -1
        self._remove(key)
        key, val = node.getData()
        self._append(key, val)
        # post-cond
        assert itemFinder[key] is not node
        return val

    def put(self, key: 'int', value: 'int') -> 'None':
        itemFinder = self.itemFinder
        node = itemFinder.get(key)
        if node:
            self._remove(key)
        if len(itemFinder) == self.cap:
            self._expire()
        self._append(key, value)
    
    def _remove(self, key):
        """removes a key in the cache"""
        itemFinder = self.itemFinder
        assert key in itemFinder
        node = itemFinder[key]
        node.detach()
        del itemFinder[key]
    
    def _append(self, key, val):
        """appends a node with (key, val) to the tail of the list"""
        itemFinder = self.itemFinder
        # pre-cond
        assert key not in itemFinder
        
        anchor = self.anchor
        oldTail = anchor.prev
        tail = ListNode(key, val)
        oldTail.next, tail.prev = tail, oldTail
        tail.next, anchor.prev = anchor, tail
        
        itemFinder[key] = tail
        # post-cond
        assert key in itemFinder and itemFinder[key].val == val
        
    def _expire(self):
        """removes the head from the list and update dict"""
        itemFinder = self.itemFinder
        oldSize = len(itemFinder)
        assert oldSize > 0
        
        anchor = self.anchor
        head = anchor.next
        assert head is not anchor
        
        key, _ = head.getData()
        self._remove(key)
        
        # post-cond
        assert len(itemFinder) == oldSize - 1
```

