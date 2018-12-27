# LRU cache

## Design

1. what data structure to maintain
2. clarify the properties of those structures
3. ensure post condition after each operation

### Implementation

1. take-away: pre-condition and post-condition

## Solutions

### with Doubly LinkedList + Hashtable

```python
class LRUCache:
    class ListNode():
        def __init__(self, key, value):
            self.key, self.val = key, value
            self.prev, self.next = None, None
            
        def detach(self):
            # bug: wrongly value set
            prev, nxt = self.prev, self.next
            prev.next, self.prev = nxt, None
            self.next, nxt.prev  = None, prev

    def __init__(self, capacity):
        self.capacity = capacity
        self.nodeFinder = {} # {int: ListNode}
        # prev: tail, next: head; head: least recently used, tail: most recently used
        anchor = self.anchor = self.ListNode(-1, -1) # the anchor of a `cyclic` doubly linkedlist
        anchor.prev, anchor.next = anchor, anchor # missing

    def get(self, key):
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

```

### with Priority Queue + Hash-table

```python
from heapq import heappush, heappop
from itertools import count
class LRUCache(object):
    # maintain a priority queue with the time as priority
    # an item can be invalidated in the heap
    # shape: [timestamp, ([key,val]|<placehoder>)], ex. [2, [1, 1]], [5, <placeholder>]
    
    def __init__(self, capacity):
        self.pq = [] # item type: [timestamp, [key, val]], designed to be mutable
        self.itemFinder = {} # {key: heap item}, size of this map is the size of the cache
        self.capacity = capacity
        self.counter = count()
        self.PLACE_HOLDER = object() # a singleton to check validity

    def get(self, key):
        itemFinder, pq = self.itemFinder, self.pq
        PLACE_HOLDER = self.PLACE_HOLDER
        size = len(itemFinder)
        
        # 1. check key
        if key not in itemFinder:
            return -1
        
        # 2. update key position in the heap: invalidate and add a new item
        kvPair = self._invalidate(key)
        newItem = [next(self.counter), kvPair]
        itemFinder[key] = newItem
        heappush(pq, newItem)
        
        # post-cond
        assert size == len(itemFinder)
        assert kvPair[0] == key
        return kvPair[1]

    def put(self, key, value):
        itemFinder, pq = self.itemFinder, self.pq
        PLACE_HOLDER = self.PLACE_HOLDER
        capacity = self.capacity
        
        # 1. check key, update
        if key in itemFinder:
            kvPair = self._invalidate(key)
            kvPair[1] = value
            newItem = [next(self.counter), kvPair]
            itemFinder[key] = newItem
            heappush(pq, newItem)
        else:
            # 2. expire (invalidate)
            if len(itemFinder) == capacity:
                self._expire()
                assert len(itemFinder) == capacity - 1
            # 3. add key
            newItem = [next(self.counter), [key, value]]
            ## invariant before adding key
            assert key not in itemFinder and len(itemFinder) < capacity
            itemFinder[key] = newItem
            heappush(pq, newItem)
        # post-cond
        assert len(itemFinder) <= capacity
        assert itemFinder[key][1][1] == value

    def _invalidate(self, key): # -> return the kvPair
        """invalidate the current item associate with the key"""
        itemFinder = self.itemFinder
        PLACE_HOLDER = self.PLACE_HOLDER
        assert key in itemFinder
        item = itemFinder[key]
        _, kvPair = item
        assert kvPair is not PLACE_HOLDER
        item[1] = PLACE_HOLDER # invalidate
        return kvPair
    
    def _expire(self):
        itemFinder, pq = self.itemFinder, self.pq
        PLACE_HOLDER = self.PLACE_HOLDER
        while pq[0][1] is PLACE_HOLDER:
            heappop(pq)
        assert len(pq) > 0
        _, (key, _) = heappop(pq)
        del itemFinder[key]
```

