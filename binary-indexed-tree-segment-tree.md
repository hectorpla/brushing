# Binary Indexed Tree \(Segment tree\)

Recursive solution

```python
class BinaryIndexTree():
    class Node:
        """internal node"""
        def __init__(self, start, end):
            self.val = 0
            self.start, self.end = start, end
            self.left, self.right = None, None

    # abstract out the opeartion
    def __init__(self, data):
        """data in 1-d array"""
        self.data = data
        self.root = self._build(0, len(data) - 1)

    def _build(self, left, right):
        """recursively build the tree"""
        if left > right:
            return None
        array = self.data
        node = self.Node(left, right)
        if left == right:
            node.val = array[left]
            return node
        mid = (left + right) // 2
        node.left = self._build(left, mid)
        node.right = self._build(mid + 1, right)
        node.val += node.left.val if node.left else 0
        node.val += node.right.val if node.right else 0
        return node

    def update(self, index, datum):
        self._update(index, datum, self.root)
        self.data[index] = datum # modify the original data

    def _update(self, index, datum, node):
        if not node:
            return
        start, end = node.start, node.end
        if start <= index <= end:
            node.val += datum - self.data[index] # change the value
            self._update(index, datum, node.left)
            self._update(index, datum, node.right)

    def query(self, start, end):
        return self._query(self.root, start, end)

    def _query(self, node, start, end):
        if not node:
            return 0
        nstart, nend = node.start, node.end
        # start <=========> end
        #    nstart <===> nend
        if nstart >= start and nend <= end:
            return node.val
        result = 0
        #   start <===.. end
        # nstart <=..> nend
        # 
        # start ..===> end
        #  nstart <..==> nend
        #if nend >= start or nstart <= end: # <<<< TLE >>>>>
        #    return self._query(node.left, start, end) + self._query(node.right, start, end)
        
        mid = (nstart + nend) // 2
        if mid >= start:
            result += self._query(node.left, start, end)
        if mid + 1 <= end:
            result += self._query(node.right, start, end)
        return result
        
class NumArray:
    
    def __init__(self, nums):
        """
        :type nums: List[int]
        """
        self.BIT = BinaryIndexTree(nums)

    def update(self, i, val):
        """
        :type i: int
        :type val: int
        :rtype: void
        """
        self.BIT.update(i, val)
        

    def sumRange(self, i, j):
        """
        :type i: int
        :type j: int
        :rtype: int
        """
        return self.BIT.query(i, j)


# Your NumArray object will be instantiated and called as such:
# obj = NumArray(nums)
# obj.update(i,val)
# param_2 = obj.sumRange(i,j)
```



