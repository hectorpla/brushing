# Iterators

```python
class BSTIterator:
    # in-order traversal
    def __init__(self, root):
        """
        :type root: TreeNode
        """
        self.stack = []
        # invariant: before any next(), hasNext() call,
        # stack is empty or the top of stack is next to pop
        self._update(root)
        # in case we want to filter some types of values, 
        # add one more layer to maintain the additional invariant
        
    def next(self):
        """
        @return the next smallest number
        :rtype: int
        """
        if not self.stack:
            raise ValueError('no next')
        node = self.stack.pop()
        self._update(node.right)  # explore the right sub-tree, maintain invariant
        return node.val

    def hasNext(self):
        """
        @return whether we have a next smallest number
        :rtype: bool
        """
        return len(self.stack) > 0
        
    def _update(self, node):
        """appends the left path all the way to the leaf"""
        while node:
            self.stack.append(node)
            node = node.left
```

