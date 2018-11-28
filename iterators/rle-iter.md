# RLE iter

a naive/un-elegant handling of invariant

```python
class RLEIterator:

    def __init__(self, A):
        """
        :type A: List[int] 18
        """
        self.array = A
        self.p = 0 # current 

    def next(self, n):
        """
        :type n: int
        :rtype: int
        """
        # invariant: p pointing a count > 0, otherwise out of boundary
        array = self.array
        m = len(array)
        while self.p < m and array[self.p] == 0:
            self.p += 2
        if self.p == m:
            return -1
        ret = -1
        while self.p < m and n:
            ret = array[self.p + 1]
            delta = min(n, array[self.p])
            array[self.p] -= delta
            n -= delta
            if array[self.p] == 0:
                self.p += 2
        return -1 if n else ret
```

{% hint style="info" %}
the invariant setting is not initially correct, misleading when coding
{% endhint %}

```python
class RLEIterator:

    def __init__(self, A):
        """
        :type A: List[int] 18
        """
        self.array = A
        self.p = 0 # current 

    def next(self, n):
        """
        :type n: int
        :rtype: int
        """
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

