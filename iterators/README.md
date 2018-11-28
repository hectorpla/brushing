# Iterators



recipe for tree-like traversal: 

* determine the type of elems in stack, can it be nullable
* invariant after calling hasNext\(\) and next\(\)
* combination of the two methods

recipe for general iterator:

* propose variables to maintain
* advance\(\) helper method and its pre- post- condition
* 
### 281. Zigzag/Cyclic Iterator

did a slow solution

### 341. Flatten Nested List Iterator

When using stack to simulate recursion, think about what is the type of the elements in the stack: in the cases of trees, it should be **Node**. In this problem it's NestedInteger

should be aware that in the stack, every nodes is in an array!!!



here, because all nodes printed are **terminal** node, traversal can be deemed as **pre-order \(the easiest case to make iterative\)**

```python
class NestedIterator(object):

    def __init__(self, nestedList):
        self.stack = list(reversed(nestedList))

    def next(self):
        if self.hasNext(): # don't know if calling hasNext() here is anti-pattern
            return self.stack.pop().getInteger()
        raise Exception('no next')
        
    def hasNext(self):
        # invariant: after calling this method, stack should be empty of the last elem is an integer
        while self.stack and not self.stack[-1].isInteger():
            self.stack.extend(reversed(self.stack.pop().getList()))
        return len(self.stack) > 0
```



### 173. Binary Search Tree Iterator

solution 1: like in iterative traversal \(spent much time\)

```python
class BSTIterator(object):
    # idea: in-order traversal, we don't know whether to pop a node or not, so we keep track of the last node popped
    
    def __init__(self, root):
        """
        :type root: TreeNode
        """
        # elems should not be nullable? no
        self.stack = [] # null case
        self.front = root

    def hasNext(self):
        self._advance()
        return not not self.stack

    def next(self):
        self._advance()
        if not self.stack:
            raise Exception('no next')
        cur = self.stack.pop()
        self.front = cur.right
        return cur.val
        
    def _advance(self):
        '''invariant: self.front is None, unstraight-forward'''
        while self.front:
            self.stack.append(self.front)
            self.front = self.front.left
```



solution 2:

inspired by 341, type of the stack

**Sub-typing** vs. **Inheritance** \(this method can't be done in class based languages like Java\)  
**Variants** \([https://ocaml.org/learn/tutorials/data\_types\_and\_matching.html](https://ocaml.org/learn/tutorials/data_types_and_matching.html)\)

```python
class BSTIterator(object):
    # idea: in-order traversal, we don't know whether to pop a node or not, so we keep track of the last node popped
    
    def __init__(self, root):
        # elems should not be nullable? no
        self.stack = [] if root is None else [root] # heterogenous list, type Elem = TreeNode | int

    def hasNext(self):
        """
        :rtype: bool
        """
        self._advance()
        return not not self.stack

    def next(self):
        self._advance()
        if not self.stack:
            raise Exception('no next')
        return self.stack.pop()
        
    def _advance(self):
        '''invariant: stack is empty or stack[-1] is of type int '''
        while self.stack and type(self.stack[-1]) is not int:
            node = self.stack.pop()
            if node.right:
                self.stack.append(node.right)
            self.stack.append(node.val)
            if node.left:
                self.stack.append(node.left)
```



### 284. Peeking Iterator

precondition before checking the next value: either the iterator is empty or the peekElem is not None 

```python
class PeekingIterator(object):
    def __init__(self, iterator):
        # assumption: contents do not include None
        self.it = iterator
        self.peekElem = None # unsafe, what if the content of the iterator can be None
        # safe alternative, set it to an unique anchor (address)

    def peek(self):
        self._advance()
        return self.peekElem

    def next(self):
        self._advance()
        ret = self.peekElem
        self.peekElem = None
        return ret
        
    def hasNext(self):
        return not not self.peekElem or self.it.hasNext()
        
    def _advance(self):
        """post-condition invariant: peekElem is not null or it has no next """
        if not self.peekElem and self.it.hasNext():
            self.peekElem = self.it.next()

```



### 251. Flatten 2D Vector

take-away: see  \_advance\(\) method

```python
class Vector2D(object):
    def __init__(self, vec2d):
        self.vecs = vec2d
        self.row = 0
        self.col = 0

    def next(self):
        self._advance()
        # error handling
        ret = self.vecs[self.row][self.col]
        self.col += 1
        return ret

    def hasNext(self):
        self._advance()
        return self.row < len(self.vecs)
        
    def _advance(self):
        # pre: row <= n, col <= len(vecs[row])
        while self.row < len(self.vecs) and self.col >= len(self.vecs[self.row]): # typo: forget the len
            self.row += 1
            self.col = 0
        # post: row == len(vecs) or col < len(vecs[row])
```

