# Iterators

## Conclusion

### invariants: the methodologies to write good OO programs \(classes\)

### recipe for nested structure, like tree traversal: 

* determine the type of elems in **stack**, can it be nullable; should maintain **another** var?
* invariant after calling hasNext\(\) and next\(\)

### **recipe for general iterator:**

* propose variables to maintain
* **invariant** before yielding the next value - advance\(\) helper method and its pre- post- condition
* decide where to keep the invariant \(sometimes inside next\(\) method, sometimes inside hasNext\(\) \)

## Problems

### 281. Zigzag/Cyclic Iterator

{% hint style="info" %}
1. variables: a queue, each item is \(vec: 'a\[\], p: int\) pair, 
2. invariant: for every pair in the queue, p is in boundary
3. when pop from a q, ensure the invariant when appending
{% endhint %}

like round-robin, using **queue**

### 341. Flatten Nested List Iterator

When using stack to simulate recursion, think about what is the type of the elements in the stack: in the cases of trees, it should be **Node**. In this problem it's NestedInteger

here, because all nodes printed are **terminal/leave** node, traversal can be deemed as **pre-order \(the easiest case to make iterative\)**

{% hint style="info" %}
1. **vars: a stack where every element is a NestedInteger**
2. **invariant before return a next val: the last elem in the stack is an Integer**
3. **maintain invariant before calling next\(\), that is, in hasNext**
{% endhint %}

```python
class NestedIterator(object):

    def __init__(self, nestedList):
        self.stack = list(reversed(nestedList))

    def next(self):
        if self.hasNext(): # don't know if calling hasNext() here is anti-pattern
            return self.stack.pop().getInteger()
        raise Exception('no next')
        
    def hasNext(self):
        # invariant: after calling this method, 
        # the last elem is an integer or stack should be empty
        while self.stack and not self.stack[-1].isInteger():
            self.stack.extend(reversed(self.stack.pop().getList()))
        return len(self.stack) > 0
```



### 173. Binary Search Tree Iterator

solution 1: like in iterative traversal \(spent much time\)

{% hint style="info" %}
1. vars: a stack, elems are tree node; a cur node awaiting to append into the stack
2. invariant before return the next val: cur is None
3. when?: before calling next\(\)
{% endhint %}

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

{% hint style="info" %}
1. vars: current value, and the original iterator
2. invariant before returning next\(\) and peek\(\): the cur is not anchor
3. when: right before the real logic inside peek\(\) or next\(\) 
{% endhint %}

```python
class PeekingIterator:
    def __init__(self, iterator):
        """
        Initialize your data structure here.
        :type iterator: Iterator
        """
        self.anchor = object()  # a singleton for checking if the peekVal is valid 
        self.peekVal = self.anchor
        self.iter = iterator

    def peek(self):
        """
        Returns the next element in the iteration without advancing the iterator.
        :rtype: int
        """
        # idempotent
        if self.peekVal is self.anchor:  # not valid
            if not self.iter.hasNext():
                raise ValueError('calling next() on the empty iterator')
            self.peekVal = self.iter.next()
        return self.peekVal
            
    def next(self):
        """
        :rtype: int
        """
        result = self.peek()
        self.peekVal = self.anchor  # make the popped value invalid
        return result

    def hasNext(self):
        """
        :rtype: bool
        """
        return self.peekVal is not self.anchor or self.iter.hasNext()
```



### 251. Flatten 2D Vector

{% hint style="info" %}
1. vars: current row and current column
2. invariant before return next: row is in bound, col is in bound
3. when: before return
{% endhint %}

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

### 604. Design Compressed String Iterator

{% hint style="info" %}
1. vars: pointer in the str, a remaining count, next char pos 
2. invariant: p pointing to a char where the remaining count &gt; 0
3. when: before next
{% endhint %}

```python
class StringIterator(object):    
    # caveat: should parse number
    def __init__(self, compressedString):
        """
        :type compressedString: str
        """
        self.string = compressedString
        self.p = 0
        self.remaining = 0
        self.nextCharPos = 0

    def next(self):
        """
        :rtype: str
        """
        # the context of this problem: don't call hasNext() before next(), should do it here
        if not self.hasNext():
            return " "
        self.remaining -= 1
        return self.string[self.p]

    def hasNext(self):
        """
        :rtype: bool
        """
        # maintain here
        n = len(self.string)
        while self.remaining == 0:
            # parse number
            self.p = self.nextCharPos
            if self.p == n:
                break
            start = i = self.p + 1
            while i < n and self.string[i].isdigit():
                i += 1
            self.nextCharPos = i
            self.remaining = int(self.string[start:i])
        return self.remaining > 0
```

