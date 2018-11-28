# Data structure design

### 155. Min Stack

run tests \(see bottom\)

```python
class MinStack:
    # maintain two parallel stacks, one for pushed element, one for the min value associated with it
    def __init__(self):
        """
        initialize your data structure here.
        """
        self.stack = []
        self.minStack = []

    def push(self, x):
        """
        :type x: int
        :rtype: void
        """
        self.stack.append(x)
        self.minStack.append(min(x, self.minStack[-1] if self.minStack else x)) # bug: stack -> minStack

    def pop(self):
        """
        :rtype: void
        """
        self.minStack.pop()
        self.stack.pop()
        
    # assume valid operations?
    def top(self):
        """
        :rtype: int
        """
        if len(self.stack) == 0:
            pass # sanity check
        return self.stack[-1]

    def getMin(self):
        """
        :rtype: int
        """
        if len(self.minStack) == 0:
            pass # sanity check
        return self.minStack[-1]
        
        
# !!! should refer to code while running test
# tests (p#: push #, pop: pop, t: top, m: getMin)
# [p1, t, m, pop, p3, p2, t, m, p5, m, t, pop, m, t, pop, m, t]
# [*,  1, 1, *,   *,   *, 3, 2]

# ops   stack       minStack    result
# p1    [1]         [1]
# t                             1
# m                             1
# pop   []          []
# p3    [3]         [3]
# p2    [3,2]       [3,2]
# t                             2
# m                             2
# p5    [3,2,5]     [3,2,2]
# m                             2
# t                             5
# pop   [3,2]       [3,2]       
# m                             2
# t                             2
# pop   [3]         [3]
# m                             3
# t                             3
```

