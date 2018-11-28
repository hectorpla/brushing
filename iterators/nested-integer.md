# Nested Integer

```python
class NestedIterator(object):

    def __init__(self, nestedList):
        """
        Initialize your data structure here.
        :type nestedList: List[NestedInteger]
        """
        self.list = nestedList
        self.stack = []

    def next(self):
        """
        :rtype: int
        """
        return self.stack.pop().getInteger()

    def hasNext(self):
        """
        :rtype: bool
        """
        # invariant: the last element is an Integer; list not empty, otherwise stack is empty
        stack = self.stack
        while self.list or (stack and not stack[-1].isInteger()):
            if self.list: # list with higher priority
                stack.extend(reversed(self.list))
                self.list = [] # bug
            else:
                self.list = stack.pop().getList()
            # print(stack, lst)
        # post: the last element is an Int
        return len(stack) > 0
```

