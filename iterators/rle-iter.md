---
description: Nov 18; Jan 19
---

# RLE iter

{% hint style="info" %}
the invariant setting is not initially correct, misleading when coding
{% endhint %}

```python
class RLEIterator:

    def __init__(self, A):
        self.array = A
        self.p = 0 # current 

    def next(self, n):
        # invariant: p pointing to either a count >= 0
        array = self.array
        m = len(array)
        ret = -1
        while self.p < m and n:
            ret = array[self.p + 1]
            delta = min(n, array[self.p])
            array[self.p] -= delta
            n -= delta
            if array[self.p] == 0:
                self.p += 2
        # post condition: n is all consumed, in which case return the last value, otherwise return -1
        return -1 if n else ret
```

before TA onsite:

```python
class RLEIterator:
    # be aware of zero count
    
    def __init__(self, A):
        # don't modify the input
        # assume len(A) is even
        self.A = A
        self.p = -2
        self.count = 0
        
        # invariant: after init() and next()
        #            count > 0 or p out of bound
        self._update()

    def next(self, n):
        assert n >= 1  # assume: n >= 1
                
        # attemp to decrease n to 0
        # loop invariant: self.count > 0 or self.p out of bound
        while n and self.p < len(self.A):
            delta = min(self.count, n)  # number to consume the current pair
            n -= delta
            self.count -= delta
            val = self.A[self.p + 1]
            self._update()  # maintain class invariant
        
        if n > 0:
            return -1  # exhaust the input
        return val
        
    def _update(self):
        while self.count == 0 and self.p < len(self.A):
            self.p += 2
            if self.p < len(self.A):
                self.count = self.A[self.p]
```

