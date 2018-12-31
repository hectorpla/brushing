---
description: 'Nov, Dec'
---

# Nested Integer

```python
class NestedIterator(object):

    def __init__(self, nestedList):
        """
        Initialize your data structure here.
        :type nestedList: List[NestedInteger]
        """
        self.stack = list(reversed(nestedList))  # type elem = NI
        # invariant:
        # after each operation, the top of the stack is a NI of integer, otherwise stack is empty
        # mind the order the elem is popped from list
        self._advance()
    
    def _advance(self): # the helper to maintain invariant
        stack = self.stack
        while stack and not stack[-1].isInteger():
            node = stack.pop()
            stack.extend(reversed(node.getList()))
    
    def next(self):
        """
        :rtype: int
        """
        stack = self.stack
        if not stack:
            raise IndexError("no next!")
        node = stack.pop()
        assert node.isInteger()
        # maintain the variant before returning
        self._advance()
        return node.getInteger()
        
    def hasNext(self):
        """
        :rtype: bool
        """
        return len(self.stack) > 0
    
# tests
# [[1,2],3,[4,5]]
# [[[]]]
# [[],1]
```

```python
class NestedIterator(object):

    def __init__(self, nestedList):
        """
        Initialize your data structure here.
        :type nestedList: List[NestedInteger]
        """
        # two variables to maintain
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

